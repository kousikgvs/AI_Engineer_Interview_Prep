# Week 9 — Interview Questions: Backend Engineering for AI Products

> Sourced from backend + AI-product engineer screens. Includes system-design prompts.

---

## APIs
1. REST design: status codes, resource modeling, idempotency — what makes an API idempotent?
2. What does FastAPI give you (async, dependency injection, Pydantic validation)?
3. WebSockets vs Server-Sent Events (SSE) — when do you use each for LLM streaming?
4. Explain the async event loop. What happens if you block it?
5. How do you version an API without breaking existing clients?

## Databases
6. How do B-tree indexes work? When does an index HURT performance?
7. What is `EXPLAIN ANALYZE` and how do you read a query plan?
8. What is connection pooling (PgBouncer) and why is it critical under load?
9. Redis caching patterns: cache-aside vs write-through vs write-behind.
10. **When would you choose Redis vs Postgres for storing session context?**
11. OLTP vs OLAP — when to use each?

## Queues & Background Jobs
12. When do you use a task queue in an AI system (async inference, batch jobs)?
13. Celery vs ARQ vs RQ vs RabbitMQ vs Kafka — tradeoffs.
14. What is a dead-letter queue and retry logic?

## Auth & Security
15. Explain JWT structure, signing, verification, and refresh tokens.
16. What is RBAC? How do you manage API keys and rate limits?

## Deployment & Distributed Systems
17. Docker multi-stage builds and layer caching — why do they matter?
18. What does Nginx do as a reverse proxy / load balancer?
19. **Explain the CAP theorem. How does it apply to a distributed agent system?**
20. What is Raft (concept level)? Eventual consistency implications?

## System Design (very common)
21. **Design an API that streams LLM responses to many concurrent users.**
22. Design the backend for a chatbot with history, caching, and rate limiting.

## Failure / Judgment
23. A production AI API went down under load — no pooling, no rate limiting, blocking event loop. Fix it.
24. "Postgres or Redis for session storage in this system?" — defend it.
