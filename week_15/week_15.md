# Week 15 — Security & Distributed Systems

**Block:** E — Evaluation + Reliability + Security  
**Goal:** Enterprise AI requires security. Scalable AI requires distributed systems.

---

## What You Will Learn

### AI Security
- Threat modeling for AI systems — mapping attack surfaces
- Prompt injection — direct, indirect, and multi-turn attacks
- Jailbreaking — techniques and defenses
- Data exfiltration via prompt injection
- RAG security — poisoning the knowledge base
- MCP (Model Context Protocol) security — tool permission boundaries
- Agent permission systems — principle of least privilege
- Adversarial examples (concept level)
- Model extraction attacks

### Red Teaming
- Red team methodology for AI systems
- Automated red teaming — using a model to attack a model
- Test cases: role confusion, instruction override, context manipulation
- Building a red team evaluation suite
- Responsible disclosure in AI

### Distributed Systems (Full Block)
- CAP theorem — deeper dive: network partitions are unavoidable
- Consistency models: strong, eventual, causal, read-your-writes
- Consensus — the problem of agreement in distributed systems
- Raft algorithm — leader election, log replication, safety guarantees
- Replication — master-replica and multi-master
- Sharding — consistent hashing and hot spots
- Event-driven architecture — events as the source of truth
- Message queues — Kafka, RabbitMQ, SQS — tradeoffs
- Saga pattern — distributed transactions in microservices
- Idempotency — designing operations that are safe to retry

### Case Studies
- OpenAI serving architecture — how they handle millions of requests
- Stripe payments — idempotency keys and exactly-once semantics
- Uber dispatch — real-time matching at scale

---

## Project
- Red-team your own agent from Week 13
  - Document 5+ successful attacks
  - Implement defenses for each attack
  - Build an automated red team eval suite
- Implement a simple distributed key-value store using Raft (simplified)
- Design the architecture for an AI system handling 10 million daily requests

---

## Paper to Read
- "Universal and Transferable Adversarial Attacks on Aligned Language Models" — Zou et al.

---

## Failure Friday
- Analyze: A real prompt injection attack in production — how was the system compromised?

---

## Interview Questions This Week Prepares You For

<details>
<summary>"What is indirect prompt injection and how do you defend against it?"</summary>

Malicious instructions hidden in content the model reads (web page, doc) that hijack its behavior. Defend by treating retrieved content as untrusted data (not instructions), sandboxing tools, least privilege, and output filtering.
</details>

<details>
<summary>"Explain the Raft consensus algorithm"</summary>

Nodes elect a leader; the leader appends commands to a replicated log and commits an entry once a majority acknowledges; followers apply committed entries. It gives consistency and tolerates minority failures with clear election/safety rules.
</details>

<details>
<summary>"Design the serving infrastructure for an LLM API handling 1M requests/day"</summary>

~12 rps average — size for peak. Load balancer → stateless API tier → batched, autoscaled GPU inference workers → Redis cache → DB. Add rate limiting, a queue for spikes, observability, and fallbacks.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"Canary deployment or blue-green for this deployment?"</strong></summary>

**What I'd do:** canary for gradual validation with real traffic; blue-green when I need an instant full switch with fast rollback.
**Why:** canary limits blast radius; blue-green minimizes downtime.
**Tradeoff:** canary rolls out slower; blue-green doubles infrastructure.
**Rejected:** blue-green when I want to catch issues on a small slice first.
</details>
