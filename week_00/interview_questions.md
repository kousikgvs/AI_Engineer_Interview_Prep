# Week 0 — Interview Questions: Python, SQL & Developer Foundations

> Click a question (▸) to expand the answer. Sourced from SWE/AI-engineer screens.

---

## Python — Core & Idioms

<details>
<summary>1. What is the difference between a <code>list</code> and a <code>tuple</code>? When would you use each?</summary>

A `list` is mutable (can grow/shrink/change); a `tuple` is immutable (fixed once created). Tuples are slightly faster, hashable (so usable as dict keys / set members), and signal "this shouldn't change." Use a list for a homogeneous, changing collection; use a tuple for a fixed record or a return of multiple values.
</details>

<details>
<summary>2. Explain mutable vs immutable types. Which built-ins are immutable?</summary>

Mutable objects can change in place (`list`, `dict`, `set`, most custom objects); immutable ones cannot (`int`, `float`, `str`, `bool`, `tuple`, `frozenset`, `bytes`). Immutability makes objects hashable and safe to share; mutable default arguments are a classic bug source.
</details>

<details>
<summary>3. What does a list comprehension compile to, and when is a generator better?</summary>

A list comprehension builds the whole list in memory eagerly. A generator expression `(x for x in ...)` is lazy — it yields one item at a time, using O(1) memory. Use a generator for large/streaming data or when you only iterate once.
</details>

<details>
<summary>4. How does Python manage memory (reference counting + GC)?</summary>

CPython frees an object when its reference count hits zero. A cyclic garbage collector additionally detects reference cycles (objects referring to each other) that ref-counting alone can't reclaim. Memory is pooled/arena-allocated for small objects.
</details>

<details>
<summary>5. Difference between <code>is</code> and <code>==</code>?</summary>

`==` compares values (calls `__eq__`); `is` compares identity (same object in memory). Use `is` only for singletons like `None`. Small ints and interned strings may share identity, which is an implementation detail — never rely on it.
</details>

<details>
<summary>6. Explain <code>*args</code> and <code>**kwargs</code>.</summary>

`*args` collects extra positional arguments into a tuple; `**kwargs` collects extra keyword arguments into a dict. They let functions accept a variable number of arguments and forward them (`f(*args, **kwargs)`).
</details>

<details>
<summary>7. What are decorators? Write one that times a function.</summary>

A decorator is a callable that wraps another function to add behavior. Example:

```python
import time, functools
def timed(fn):
    @functools.wraps(fn)
    def wrapper(*a, **k):
        t = time.perf_counter()
        r = fn(*a, **k)
        print(f"{fn.__name__} took {time.perf_counter()-t:.4f}s")
        return r
    return wrapper
```
</details>

<details>
<summary>8. What is a context manager?</summary>

An object that defines setup/teardown via `__enter__`/`__exit__`, used with `with`. It guarantees cleanup (closing files, releasing locks) even on exceptions. `contextlib.contextmanager` lets you write one as a generator with a single `yield`.
</details>

<details>
<summary>9. Shallow copy vs deep copy — where does it matter?</summary>

A shallow copy (`copy.copy`, `list[:]`) copies the outer container but shares nested objects; a deep copy (`copy.deepcopy`) recursively copies everything. It matters with nested mutable data — mutating a nested list in a shallow copy also changes the original.
</details>

<details>
<summary>10. What is the GIL and how does it affect threading vs multiprocessing?</summary>

The Global Interpreter Lock lets only one thread execute Python bytecode at a time, so threads don't speed up CPU-bound work — use multiprocessing for that. Threads still help I/O-bound work (network, disk) because the GIL is released during I/O waits.
</details>

## OOP & Clean Code

<details>
<summary>11. Inheritance vs composition — when prefer composition?</summary>

Inheritance models "is-a"; composition models "has-a." Prefer composition when you want to reuse behavior without a strict type hierarchy — it's more flexible and avoids fragile deep hierarchies ("favor composition over inheritance").
</details>

<details>
<summary>12. What are <code>@dataclass</code> and <code>@property</code> for?</summary>

`@dataclass` auto-generates `__init__`, `__repr__`, `__eq__` from typed fields, cutting boilerplate for data-holding classes. `@property` exposes a method as an attribute, enabling computed values and validation while keeping a clean `obj.attr` interface.
</details>

<details>
<summary>13. Explain <code>__init__</code> vs <code>__new__</code>.</summary>

`__new__` creates and returns the instance (rarely overridden — used for immutables or singletons); `__init__` initializes the already-created instance. `__new__` runs first.
</details>

<details>
<summary>14. What are dunder/magic methods? Name five.</summary>

Special methods with double underscores that hook into language operators: `__init__`, `__repr__`, `__eq__`, `__len__`, `__iter__`, `__getitem__`, `__call__`. They let your objects behave like built-ins.
</details>

<details>
<summary>15. What is duck typing?</summary>

"If it walks like a duck and quacks like a duck…" — Python cares about whether an object supports the needed methods/behavior, not its declared type. This enables flexible, protocol-based code.
</details>

## Error Handling, Typing & Pydantic

<details>
<summary>16. How does <code>try/except/else/finally</code> work? When does <code>finally</code> not run?</summary>

`try` runs code; `except` catches matching exceptions; `else` runs only if no exception; `finally` always runs for cleanup. `finally` won't run only if the process is killed hard (e.g., `os._exit`, power loss, or a crash in the interpreter).
</details>

<details>
<summary>17. Write a retry with exponential backoff.</summary>

```python
import time, random
def retry(fn, tries=5, base=0.5):
    for i in range(tries):
        try:
            return fn()
        except Exception:
            if i == tries - 1: raise
            time.sleep(base * (2 ** i) + random.random() * 0.1)  # jitter
```
</details>

<details>
<summary>18. Why use type hints in a dynamic language?</summary>

They document intent, enable IDE autocomplete and static checkers (mypy/pyright) to catch bugs before runtime, and power tools like Pydantic and FastAPI. They don't affect runtime behavior unless you enforce them.
</details>

<details>
<summary>19. Why validate LLM output with Pydantic instead of a raw dict?</summary>

LLM output is untrusted text — Pydantic parses and validates it against a typed schema, coercing types, catching missing/invalid fields, and giving clear errors. A raw dict silently accepts malformed data that then breaks downstream code.
</details>

<details>
<summary>20. <code>Optional[X]</code> vs <code>Union[X, None]</code>?</summary>

They're identical — `Optional[X]` is just shorthand for `Union[X, None]`. It means "an X or None." It does NOT mean the argument is optional/has a default.
</details>

## Async Python

<details>
<summary>21. Explain <code>async</code>/<code>await</code> and the event loop.</summary>

`async def` defines a coroutine; `await` suspends it until an awaitable completes, yielding control back to the event loop, which runs other ready coroutines meanwhile. It's cooperative single-threaded concurrency for I/O-bound work.
</details>

<details>
<summary>22. Concurrency vs parallelism — which does asyncio give?</summary>

Concurrency = managing many tasks that make progress by interleaving; parallelism = running them literally simultaneously on multiple cores. asyncio gives concurrency (single thread); true parallelism needs multiprocessing or multiple machines.
</details>

<details>
<summary>23. When does async help an LLM/RAG app, and when not?</summary>

It helps for I/O-bound work — many concurrent API calls, DB queries, streaming responses to users — where you're mostly waiting. It doesn't help CPU-bound work (local embedding math, heavy parsing); that needs processes/threads or a GPU.
</details>

<details>
<summary>24. What is a coroutine? How to run many concurrently?</summary>

A coroutine is a suspendable function created by `async def`. Run many with `await asyncio.gather(*coros)` (wait for all) or `asyncio.as_completed` (process as each finishes); use a `Semaphore` to bound concurrency.
</details>

## SQL

<details>
<summary>25. INNER vs LEFT vs RIGHT vs FULL OUTER JOIN?</summary>

INNER keeps only matching rows in both tables. LEFT keeps all left rows (NULLs where no match). RIGHT keeps all right rows. FULL OUTER keeps all rows from both, NULLs where no match. Example: LEFT JOIN customers→orders lists every customer even with zero orders.
</details>

<details>
<summary>26. What is a CTE vs a subquery?</summary>

A CTE (`WITH name AS (...)`) is a named, readable temporary result you can reference (and self-reference for recursion). It's functionally like a subquery but improves readability and lets you reuse the block within one query.
</details>

<details>
<summary>27. Window function to rank rows within each group?</summary>

```sql
SELECT dept, name, salary,
       ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC) AS rnk
FROM employees;
```
`PARTITION BY` resets the ranking per group; `ORDER BY` sets the ranking order.
</details>

<details>
<summary>28. <code>ROW_NUMBER</code> vs <code>RANK</code> vs <code>DENSE_RANK</code>?</summary>

ROW_NUMBER gives unique sequential numbers (no ties). RANK gives ties the same number but skips the next (1,1,3). DENSE_RANK ties without skipping (1,1,2).
</details>

<details>
<summary>29. <code>WHERE</code> vs <code>HAVING</code>?</summary>

`WHERE` filters rows before grouping/aggregation; `HAVING` filters groups after aggregation. Use `HAVING` for conditions on aggregates like `HAVING COUNT(*) > 5`.
</details>

<details>
<summary>30. Find the 2nd highest salary per department.</summary>

```sql
SELECT dept, salary FROM (
  SELECT dept, salary,
         DENSE_RANK() OVER (PARTITION BY dept ORDER BY salary DESC) r
  FROM employees) t
WHERE r = 2;
```
</details>

<details>
<summary>31. What does an index do, and when can it hurt?</summary>

An index speeds up reads by letting the DB find rows without scanning the whole table (usually a B-tree). It hurts writes (every insert/update maintains the index) and wastes space; too many indexes or indexing low-cardinality columns can slow the system.
</details>

## Tooling & Systems

<details>
<summary>32. <code>git merge</code> vs <code>git rebase</code>?</summary>

Merge creates a merge commit preserving history as-is (non-destructive). Rebase replays your commits on top of another branch for a linear history (rewrites commit hashes). Rebase for clean local history; merge for shared/public branches.
</details>

<details>
<summary>33. How do you resolve a merge conflict?</summary>

Git marks conflicting regions with `<<<<<<< ======= >>>>>>>`. You edit the file to the desired result, remove the markers, `git add` the file, then complete the merge/rebase. Tools like VS Code's merge editor help.
</details>

<details>
<summary>34. What is a virtual environment and why use it?</summary>

An isolated Python environment (`venv`) with its own installed packages, so projects don't share/conflict dependencies. It makes builds reproducible and keeps the system Python clean.
</details>

<details>
<summary>35. What is Docker and why containerize?</summary>

Docker packages an app with its dependencies and environment into a portable image that runs identically anywhere. It eliminates "works on my machine," enables reproducible deploys, and isolates services.
</details>

<details>
<summary>36. How do you keep secrets out of source control?</summary>

Store them in environment variables / a `.env` file that's git-ignored, or a secrets manager (AWS Secrets Manager, Vault). Never hardcode keys; scan repos for leaks and rotate any key that was committed.
</details>

## Coding Exercises

<details>
<summary>37. Reverse a linked list / detect a cycle.</summary>

Reverse: iterate with three pointers (prev, cur, next), flipping `cur.next = prev`. Detect a cycle with Floyd's tortoise-and-hare: a slow (1 step) and fast (2 steps) pointer meet if and only if there's a cycle.
</details>

<details>
<summary>38. Two-sum, group anagrams, valid parentheses.</summary>

Two-sum: hash map of value→index, look for `target - x` in O(n). Group anagrams: key each word by its sorted letters (or char count) into a dict. Valid parentheses: push opens onto a stack, pop and match on closes; valid if stack ends empty.
</details>

<details>
<summary>39. Parse a JSON API response into a clean schema.</summary>

Load with `json.loads`, then map into a Pydantic model (or dataclass) that declares the fields and types you actually need, dropping/renaming the rest. This validates the payload and gives typed, safe access downstream.
</details>

<details>
<summary>40. Build a CLI that calls an LLM API and returns validated JSON.</summary>

Read input args, load the API key from env, call the model requesting JSON output, parse the response into a Pydantic model, and print the validated object (or a clear error). Add retries with backoff around the network call.
</details>

## Behavioral / Judgment

<details>
<summary>41. "Sync or async for this AI workload?"</summary>

Choose async when the work is I/O-bound and concurrent — e.g., a server making many simultaneous LLM/DB calls, where async raises throughput without extra threads. Choose sync for simple scripts or CPU-bound work where async adds complexity for no gain. Tradeoff: async scales I/O concurrency but is harder to debug; the rejected alternative (threads) carries GIL and race-condition overhead for mostly-waiting workloads.
</details>

<details>
<summary>42. Tell me about a time you shipped something end-to-end quickly.</summary>

Structure with STAR: the Situation/need, the Task you owned, the Actions (scoping an MVP, cutting non-essentials, deploying fast, iterating on feedback), and the Result (shipped, measurable impact). Emphasize bias-to-ship and learning from real users.
</details>
