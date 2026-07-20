# Week 0 — Answers with Code: Python, SQL & Developer Foundations

> Detailed answers with runnable code. Easy items get a short note; tricky ones get a deeper explanation.

---

## Python — Core & Idioms

### 1. list vs tuple
*Easy.* List = mutable, tuple = immutable and hashable.

```python
nums = [1, 2, 3]      # can change
nums.append(4)
point = (10, 20)      # fixed; usable as a dict key / set member
grid = {point: "origin"}
```
Use a tuple for fixed records and dict keys; a list for changing collections.

### 2. Mutable vs immutable
*Tricky — mutable default args are a classic bug.*

```python
# BUG: the default list is created ONCE and shared across calls
def add(item, bucket=[]):
    bucket.append(item)
    return bucket

print(add(1))  # [1]
print(add(2))  # [1, 2]  <-- surprise!

# FIX: use None sentinel
def add(item, bucket=None):
    bucket = [] if bucket is None else bucket
    bucket.append(item)
    return bucket
```
Immutable built-ins: `int, float, str, bool, tuple, frozenset, bytes`.

### 3. Comprehension vs generator
*Easy.* List builds everything in memory; generator is lazy (O(1) memory).

```python
squares = [x*x for x in range(1_000_000)]     # ~8MB in RAM
squares_gen = (x*x for x in range(1_000_000)) # lazy, one at a time
```

### 4. Memory management
*Simple.* CPython frees objects when their reference count hits 0; a cyclic GC reclaims reference cycles.

```python
import sys, gc
a = []
print(sys.getrefcount(a))  # refs to `a`
gc.collect()               # force cyclic collection
```

### 5. is vs ==
*Easy.* `==` compares value; `is` compares identity.

```python
a = [1, 2]; b = [1, 2]
print(a == b)  # True (same value)
print(a is b)  # False (different objects)
x = None
print(x is None)  # correct way to test None
```

### 6. *args / **kwargs
```python
def f(*args, **kwargs):
    print(args)    # tuple of positionals
    print(kwargs)  # dict of keyword args
f(1, 2, mode="fast")  # (1, 2) {'mode': 'fast'}
```

### 7. Decorators — timing
```python
import time, functools
def timed(fn):
    @functools.wraps(fn)          # preserve name/docstring
    def wrapper(*a, **k):
        t = time.perf_counter()
        try:
            return fn(*a, **k)
        finally:
            print(f"{fn.__name__} took {time.perf_counter()-t:.4f}s")
    return wrapper

@timed
def work(): time.sleep(0.1)
work()
```

### 8. Context manager
```python
from contextlib import contextmanager

class Timer:                       # class form
    def __enter__(self):
        self.t = __import__("time").perf_counter(); return self
    def __exit__(self, *exc):
        print("elapsed", __import__("time").perf_counter() - self.t)

@contextmanager                    # generator form
def open_upper(path):
    f = open(path)
    try:
        yield f
    finally:
        f.close()
```
`__exit__` runs even on exceptions — guaranteeing cleanup.

### 9. Shallow vs deep copy
*Tricky — nested mutables.*

```python
import copy
a = [[1, 2], [3, 4]]
shallow = copy.copy(a)      # or a[:]
deep = copy.deepcopy(a)
a[0][0] = 99
print(shallow[0][0])  # 99  <-- shares nested list
print(deep[0][0])     # 1   <-- fully independent
```

### 10. GIL
*Detailed — common misconception.* The Global Interpreter Lock allows only one thread to run Python bytecode at once, so CPU-bound code doesn't speed up with threads. It's released during I/O, so threads still help I/O-bound work. For CPU parallelism, use processes.

```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor
# I/O-bound -> threads help
with ThreadPoolExecutor() as ex: ex.map(fetch_url, urls)
# CPU-bound -> processes bypass the GIL
with ProcessPoolExecutor() as ex: ex.map(crunch_numbers, chunks)
```

---

## OOP & Clean Code

### 11. Inheritance vs composition
```python
# Composition ("has-a") — flexible, preferred
class Engine:
    def start(self): return "vroom"
class Car:
    def __init__(self): self.engine = Engine()
    def start(self): return self.engine.start()
```

### 12. @dataclass and @property
```python
from dataclasses import dataclass
@dataclass
class User:
    name: str
    age: int
    @property
    def is_adult(self) -> bool:   # computed attribute
        return self.age >= 18
u = User("Ana", 20); print(u.is_adult)  # True
```

### 13. __init__ vs __new__
*Tricky.* `__new__` creates the instance; `__init__` initializes it.

```python
class Singleton:
    _inst = None
    def __new__(cls):
        if cls._inst is None:
            cls._inst = super().__new__(cls)
        return cls._inst
print(Singleton() is Singleton())  # True
```

### 14. Dunder methods
```python
class Vec:
    def __init__(self, x, y): self.x, self.y = x, y
    def __repr__(self): return f"Vec({self.x},{self.y})"
    def __add__(self, o): return Vec(self.x+o.x, self.y+o.y)
    def __eq__(self, o): return (self.x, self.y) == (o.x, o.y)
    def __len__(self): return 2
print(Vec(1,2) + Vec(3,4))  # Vec(4,6)
```

### 15. Duck typing
*Easy.* Python cares about behavior, not type.

```python
def total(seq):           # works on any iterable of numbers
    return sum(seq)
total([1,2,3]); total((1,2)); total({1,2})
```

---

## Error Handling, Typing & Pydantic

### 16. try/except/else/finally
```python
try:
    x = risky()
except ValueError as e:
    handle(e)
else:
    use(x)          # only if no exception
finally:
    cleanup()       # always (except hard kill / os._exit)
```

### 17. Retry with exponential backoff
*Tricky — needs jitter.*

```python
import time, random
def retry(fn, tries=5, base=0.5, cap=10):
    for i in range(tries):
        try:
            return fn()
        except Exception:
            if i == tries - 1:
                raise
            sleep = min(cap, base * (2 ** i)) + random.random() * 0.1
            time.sleep(sleep)   # jitter prevents thundering herd
```

### 18. Type hints
*Easy.* Documentation + IDE/mypy checks; no runtime effect by default.

```python
def greet(name: str, times: int = 1) -> str:
    return (f"hi {name}\n") * times
```

### 19. Pydantic for LLM output
*Detailed — key production pattern.*

```python
from pydantic import BaseModel, ValidationError
class Ticket(BaseModel):
    category: str
    priority: int
    summary: str

raw = '{"category":"billing","priority":"2","summary":"refund"}'
try:
    t = Ticket.model_validate_json(raw)  # coerces "2" -> 2, validates
    print(t.priority + 1)                # safe typed access
except ValidationError as e:
    print("bad LLM output:", e)
```
Raw `dict` parsing would silently accept malformed data that breaks later.

### 20. Optional vs Union
*Easy.* Identical: `Optional[X] == Union[X, None]`.

---

## Async Python

### 21–24. async/await, concurrency, coroutines
*Detailed.*

```python
import asyncio, httpx

async def fetch(client, url):
    r = await client.get(url)     # suspends here, loop runs others
    return r.status_code

async def main(urls):
    async with httpx.AsyncClient() as client:
        # run many coroutines concurrently
        results = await asyncio.gather(*(fetch(client, u) for u in urls))
    return results

# asyncio.run(main([...]))
```
- **Concurrency vs parallelism:** asyncio gives concurrency on one thread (great for I/O-bound LLM/DB calls); use processes for CPU parallelism.
- **Bounded concurrency:** wrap calls in `asyncio.Semaphore(n)` to cap simultaneous requests.

---

## SQL

### 25. Joins
```sql
-- customers with (or without) orders
SELECT c.id, c.name, o.total
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.id;  -- keeps customers with 0 orders
```
INNER = matches only; LEFT = all left; RIGHT = all right; FULL OUTER = all rows.

### 26. CTE vs subquery
```sql
WITH ranked AS (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC) r
  FROM employees
)
SELECT * FROM ranked WHERE r <= 3;  -- top 3 per dept, readable + reusable
```

### 27 & 28. Window functions & ranking
*Tricky — know the three ranking funcs.*

```sql
SELECT name, dept, salary,
  ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC) AS rn,  -- 1,2,3,4
  RANK()       OVER (PARTITION BY dept ORDER BY salary DESC) AS rk,  -- 1,1,3,4
  DENSE_RANK() OVER (PARTITION BY dept ORDER BY salary DESC) AS dr   -- 1,1,2,3
FROM employees;
```

### 29. WHERE vs HAVING
```sql
SELECT dept, COUNT(*) FROM employees
WHERE active = true          -- filters rows BEFORE grouping
GROUP BY dept
HAVING COUNT(*) > 5;         -- filters groups AFTER aggregation
```

### 30. 2nd highest salary per department
```sql
SELECT dept, salary FROM (
  SELECT dept, salary,
         DENSE_RANK() OVER (PARTITION BY dept ORDER BY salary DESC) r
  FROM employees
) t
WHERE r = 2;
```

### 31. Indexes
*Simple.* Speed reads (B-tree, O(log n)); cost writes and space. Avoid on low-cardinality columns.

```sql
CREATE INDEX idx_orders_customer ON orders(customer_id);
EXPLAIN ANALYZE SELECT * FROM orders WHERE customer_id = 42;
```

---

## Tooling & Systems

### 32–36. Git, venv, Docker, secrets
```bash
git switch -c feature      # branch
git rebase main            # linear history (local only)
python -m venv .venv && . .venv/Scripts/activate   # isolated env
```
```dockerfile
# multi-stage build keeps the runtime image small
FROM python:3.12 AS build
COPY requirements.txt .
RUN pip install --user -r requirements.txt
FROM python:3.12-slim
COPY --from=build /root/.local /root/.local
COPY . /app
```
Keep secrets in a git-ignored `.env` (or a secrets manager); never commit keys.

---

## Coding Exercises

### 37. Reverse a linked list / detect a cycle
```python
def reverse(head):
    prev = None
    while head:
        head.next, prev, head = prev, head, head.next
    return prev

def has_cycle(head):                 # Floyd's tortoise & hare
    slow = fast = head
    while fast and fast.next:
        slow, fast = slow.next, fast.next.next
        if slow is fast:
            return True
    return False
```

### 38. Two-sum / group anagrams / valid parens
```python
def two_sum(nums, target):
    seen = {}
    for i, x in enumerate(nums):
        if target - x in seen:
            return [seen[target - x], i]
        seen[x] = i

def group_anagrams(words):
    from collections import defaultdict
    g = defaultdict(list)
    for w in words:
        g["".join(sorted(w))].append(w)
    return list(g.values())

def valid(s):
    pairs = {')':'(', ']':'[', '}':'{'}
    st = []
    for c in s:
        if c in pairs:
            if not st or st.pop() != pairs[c]:
                return False
        else:
            st.append(c)
    return not st
```

### 39 & 40. Parse JSON → schema; CLI AI tool
```python
import json, os
from pydantic import BaseModel

class Answer(BaseModel):
    text: str
    confidence: float

def call_llm(prompt: str) -> Answer:
    # pseudo: resp = client.chat(...) requesting JSON output
    resp = '{"text":"hello","confidence":0.9}'
    return Answer.model_validate_json(resp)

if __name__ == "__main__":
    import sys
    print(call_llm(sys.argv[1]))
```

---

## Behavioral / Judgment

### 41. Sync vs async for an AI workload
Choose **async** for an I/O-bound server making many concurrent LLM/DB calls; **sync** for simple scripts or CPU-bound work. Tradeoff: async scales I/O concurrency but is harder to debug; threads were rejected because the GIL limits them for CPU work and they add race-condition risk for mostly-waiting workloads.

### 42. "Shipped end-to-end fast" (STAR)
Situation → Task → Action (scoped an MVP, cut scope, deployed early, iterated on feedback) → Result (shipped, measurable impact). Emphasize bias-to-ship and learning from real users.
