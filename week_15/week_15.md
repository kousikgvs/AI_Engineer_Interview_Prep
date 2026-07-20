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
- "What is indirect prompt injection and how do you defend against it?"
- "Explain the Raft consensus algorithm"
- "Design the serving infrastructure for an LLM API handling 1M requests/day"

---

## Engineering Judgment Question
**"Canary deployment or blue-green for this deployment?"**  
Write your answer covering: what you would do, why, what tradeoff you are making, and what alternative you rejected.
