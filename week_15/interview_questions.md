# Week 15 — Interview Questions: Security & Distributed Systems

> Sourced from enterprise-AI security + distributed-systems screens. Two topics, both heavily tested.

---

## AI Security
1. What is threat modeling for an AI system? Map the attack surfaces.
2. **What is indirect prompt injection and how do you defend against it?**
3. Direct vs indirect vs multi-turn prompt injection.
4. What is jailbreaking and what defenses exist?
5. How does data exfiltration via prompt injection work?
6. What is RAG knowledge-base poisoning?
7. What is MCP security / tool permission boundaries? Principle of least privilege for agents.
8. What are adversarial examples and model-extraction attacks?

## Red Teaming
9. What is a red-team methodology for AI systems?
10. What is automated red teaming (model attacking a model)?
11. Test cases: role confusion, instruction override, context manipulation — give examples.
12. What is responsible disclosure in AI?

## Distributed Systems
13. **Explain the CAP theorem.** Why are network partitions not optional?
14. Consistency models: strong vs eventual vs causal vs read-your-writes.
15. **Explain the Raft consensus algorithm** (leader election, log replication, safety).
16. Replication: master-replica vs multi-master.
17. Sharding: consistent hashing and hot spots.
18. What is the Saga pattern for distributed transactions?
19. Why is idempotency essential in retryable operations?
20. Kafka vs RabbitMQ vs SQS — tradeoffs.

## Case Studies / System Design
21. How does OpenAI serve millions of requests? How do Stripe idempotency keys work?
22. **Design the serving infrastructure for an LLM API handling 1M requests/day.**
23. **Design an AI system that handles 10 million daily requests.**

## Failure / Judgment
24. A real prompt-injection exploit compromised a production system — how?
25. "Canary or blue-green for this deployment?" — defend it.
