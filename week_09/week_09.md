# Week 9 — Backend Engineering for AI Products

**Block:** C — Systems + Backend + Data Engineering  
**Goal:** AI engineers who cannot build software are unemployable. Fix that.

---

## What You Will Learn

### APIs
- REST API design — resource modeling, status codes, idempotency
- FastAPI — async Python, dependency injection, Pydantic validation
- WebSockets — real-time streaming for LLM outputs
- Server-Sent Events (SSE) — streaming tokens to the frontend
- Async Python — event loop, async/await, handling concurrent requests
- API versioning and backward compatibility

### Databases
- Postgres internals — B-tree indexes, query planner, EXPLAIN ANALYZE
- Indexes — when to use them, when they hurt performance
- Connection pooling with PgBouncer
- Redis — caching patterns: cache-aside, write-through, write-behind
- Redis pub/sub for real-time features
- OLTP vs OLAP — when to use each

### Task Queues and Background Jobs
- Celery — distributed task queue
- ARQ — async task queue for Python
- Message brokers: Redis queues (RQ), RabbitMQ, Kafka, Celery — tradeoffs
- When to use task queues in AI systems (async inference, batch jobs)
- Dead letter queues and retry logic
- Job prioritization

### Auth and Security
- JWT — structure, signing, verification, refresh tokens
- RBAC — role-based access control
- API key management
- Rate limiting strategies

### Deployment
- Docker — Dockerfile, multi-stage builds, layer caching
- Docker Compose — multi-service local development
- Nginx — reverse proxy, load balancing, SSL termination
- Cloud basics: AWS EC2, S3, Lambda; Vercel; Railway; Render
- Environment management and secrets handling
- Health checks and graceful shutdown

> Deep dive: full AWS infrastructure (VPC, IAM, EKS, RDS, Lambda, migration) is covered in the Advanced Depth Track — [Week 19](../week_19/week_19.md) and [Week 20](../week_20/week_20.md).

### Distributed Systems (First Look)
- CAP theorem — Consistency, Availability, Partition tolerance
- Consensus algorithms — Raft (concept level)
- Eventual consistency and its implications
- Replication strategies and sharding

---

## Project
- Build a production-grade AI API with FastAPI
- Stream LLM responses via WebSockets
- Postgres backend with proper indexing
- Redis caching layer for repeated queries
- Celery for background embedding generation
- Dockerize and deploy to Railway or Render
- Add JWT auth and rate limiting

---

## Paper to Read
- "Designing Data-Intensive Applications" — Chapter 1 (Kleppmann) — reliable, scalable, maintainable systems

---

## Failure Friday
- Analyze: A production AI API that went down under load — no connection pooling, no rate limiting, blocking event loop

---

## Interview Questions This Week Prepares You For

<details>
<summary>"Design an API that streams LLM responses to many concurrent users"</summary>

Async FastAPI with SSE/WebSocket streaming, non-blocking model calls (or a queue to inference workers), Redis for caching/rate limiting, DB connection pooling, autoscaling behind a load balancer, plus timeouts and backpressure.
</details>

<details>
<summary>"When would you choose Redis vs Postgres for storing session context?"</summary>

Redis for hot, ephemeral, low-latency session state with TTL eviction; Postgres for durable, queryable, relational data. Commonly Redis caches in front of Postgres.
</details>

<details>
<summary>"How does CAP theorem apply to a distributed agent system?"</summary>

Under a network partition you choose consistency or availability per component — serve possibly-stale agent state (AP) or refuse until consistent (CP) based on how much correctness each action needs.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"Postgres or Redis for session storage in this system?"</strong></summary>

**What I'd do:** Redis for fast ephemeral sessions with TTL; Postgres if sessions must be durable/queryable/audited.
**Why:** Redis gives sub-ms reads and easy expiry.
**Tradeoff:** Redis is in-memory and less durable.
**Rejected:** Postgres-only when latency matters and data is short-lived.
</details>
