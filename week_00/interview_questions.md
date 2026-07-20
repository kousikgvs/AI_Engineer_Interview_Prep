# Week 0 — Interview Questions: Python, SQL & Developer Foundations

> Common questions sourced from SWE/AI-engineer screens (Glassdoor, LeetCode discuss, HackerRank, real phone screens). Grouped by theme. Practice answering out loud and in code.

---

## Python — Core & Idioms
1. What is the difference between a `list` and a `tuple`? When would you use each?
2. Explain mutable vs immutable types in Python. Which built-ins are immutable?
3. What does a list comprehension compile to, and when is a generator expression better?
4. How does Python manage memory? What is reference counting and the garbage collector?
5. What is the difference between `is` and `==`?
6. Explain `*args` and `**kwargs`.
7. What are decorators? Write one that times a function.
8. What is a context manager? Implement one with `__enter__`/`__exit__` and with `contextlib`.
9. Shallow copy vs deep copy — show an example where it matters.
10. What is the GIL and how does it affect multithreading vs multiprocessing?

## OOP & Clean Code
11. Inheritance vs composition — when do you prefer composition?
12. What are `@dataclass` and `@property` used for?
13. Explain `__init__` vs `__new__`.
14. What are dunder/magic methods? Name five useful ones.
15. What is duck typing?

## Error Handling, Typing & Pydantic
16. How does `try/except/else/finally` work? When does `finally` NOT run?
17. Write a function that retries an API call with exponential backoff.
18. Why use type hints if Python is dynamically typed?
19. Why validate LLM output with Pydantic instead of parsing a raw dict?
20. Difference between `Optional[X]` and `Union[X, None]`?

## Async Python
21. Explain `async`/`await` and the event loop.
22. Difference between concurrency and parallelism. Which does asyncio give you?
23. When would async help an LLM/RAG app, and when would it not?
24. What is a coroutine? How do you run many coroutines concurrently (`gather`, `as_completed`)?

## SQL
25. Explain INNER vs LEFT vs RIGHT vs FULL OUTER JOIN with an example.
26. What is a CTE and how does it differ from a subquery?
27. Write a query using a window function to rank rows within each group.
28. Difference between `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()`.
29. `WHERE` vs `HAVING` — when do you use each?
30. Find the 2nd highest salary per department (classic).
31. What does an index do, and when can it hurt performance?

## Tooling & Systems
32. Explain the difference between `git merge` and `git rebase`.
33. How do you resolve a merge conflict?
34. What is a virtual environment and why use one?
35. What is Docker and why containerize an app?
36. How do you keep secrets/API keys out of source control?

## Coding Exercises (whiteboard/take-home)
37. Reverse a linked list / detect a cycle.
38. Two-sum, group anagrams, valid parentheses.
39. Parse a JSON API response and reshape it into a clean schema.
40. Build a small CLI that calls an LLM API and returns validated JSON.

## Behavioral / Judgment
41. "Sync or async for this AI workload?" — walk through your decision and tradeoffs.
42. Tell me about a time you shipped something end-to-end quickly.
