# Week 0 — Python, SQL & Developer Foundations

**Block:** Foundation (Pre-Sprint)
**Goal:** Build the software engineering base the rest of the sprint assumes. An AI engineer who cannot write clean Python, query a database, or call an API is unemployable.

> Why this week exists: The 18-week core sprint starts at math. But real hiring rubrics (OpenAI, SF startups) weigh Software Engineering and Shipping Ability heavily. This week closes that gap before Week 1.

---

## What You Will Learn

### Python Fundamentals
- Variables, data types, strings, numbers, booleans
- Lists, tuples, sets, dictionaries; nested structures and JSON-like data
- Loops, conditionals, functions, return values, scope, imports, modules
- List/dict comprehensions; sorting, filtering, mapping, reducing
- Handling `None`, optional values, and defaults

### Object-Oriented & Clean Python
- Classes, objects, constructors, instance methods
- Inheritance vs composition — when to use each
- Dataclasses
- When OOP helps in AI/backend apps (and when it hurts)

### Error Handling, Logging & Debugging
- Exceptions, `try/except`, custom exceptions
- Logging basics and clean error messages
- Debugging common Python issues

### Type Hints & Pydantic
- Type hints: `str`, `int`, `float`, `bool`, `list`, `dict`, `Optional`, `Union`
- Function signatures with type hints
- Pydantic models for request/response validation
- Why type safety matters in AI apps (structured LLM outputs)

### Async Python
- `async` / `await`, coroutines, running concurrent tasks
- Where async matters in LLM, RAG, and agent apps

### Python for APIs & AI Workflows
- HTTP basics; calling APIs with `requests` and async with `httpx`
- Handling API keys securely (`.env`, secrets)
- Parsing OpenAI / Gemini / Claude-style responses
- Building reusable utility functions and simple CLI AI tools

### SQL & Data Handling
- SELECT, WHERE, GROUP BY, ORDER BY
- Joins (inner, left, right, full)
- CTEs (Common Table Expressions)
- Window functions (ROW_NUMBER, RANK, LAG/LEAD)
- Pandas & NumPy basics; loading datasets, inspection, basic plotting

### Developer Tooling
- Git & GitHub — branching, commits, PRs
- Linux command line basics
- Virtual environments, `pip`, project structure
- Docker (first exposure — full depth in Week 9)

---

## Project
- Build a CLI AI tool: takes text, calls an LLM API, returns a structured (Pydantic-validated) JSON result
- Write a small SQL analytics script over a real dataset using joins, CTEs, and window functions
- Package it: clean repo structure, `.env` for secrets, README, and Git history with meaningful commits

---

## Paper / Reading
- Read: "The Twelve-Factor App" (methodology for building clean, deployable software)

---

## Failure Friday
- Analyze: An AI script that leaked an API key to a public GitHub repo — how it happened, blast radius, and how secrets management prevents it.

---

## Interview Questions This Week Prepares You For
- "Write a function that retries an API call with exponential backoff"
- "What is the difference between a list and a tuple, and when would you use each?"
- "Write a SQL query using a window function to rank rows within a group"
- "Why use Pydantic for LLM outputs instead of raw dict parsing?"

---

## Engineering Judgment Question
**"Sync or async for this AI workload?"**
Write your answer covering: what you would do, why, what tradeoff you are making, and what alternative you rejected.
