# Week 15 — Interview Questions: Security & Distributed Systems

> Click a question (▸) to expand the answer.

---

## AI Security

<details>
<summary>1. Threat modeling for an AI system?</summary>

Systematically map attack surfaces (prompts, retrieved content, tools, training data, model outputs), identify threats (injection, exfiltration, poisoning), and design controls (validation, least privilege, monitoring).
</details>

<details>
<summary>2. What is indirect prompt injection; defense?</summary>

Malicious instructions hidden in content the model later reads (a web page, doc, email) that hijack its behavior. Defend by treating retrieved content as untrusted data (not instructions), sandboxing tools, and least privilege.
</details>

<details>
<summary>3. Direct vs indirect vs multi-turn injection?</summary>

Direct: user types the attack. Indirect: attack embedded in external content the model consumes. Multi-turn: the attack is built up across several messages to bypass filters.
</details>

<details>
<summary>4. What is jailbreaking; defenses?</summary>

Prompts that trick the model into bypassing safety (role-play, obfuscation, "ignore instructions"). Defenses: robust system prompts, safety classifiers/guard models, and refusal training.
</details>

<details>
<summary>5. Data exfiltration via prompt injection?</summary>

An injected instruction makes the agent leak secrets/context (e.g., encode data into a URL it fetches). Prevent with output filtering, egress controls, and not exposing secrets to the model.
</details>

<details>
<summary>6. RAG knowledge-base poisoning?</summary>

An attacker inserts malicious documents into the corpus so retrieval surfaces attacker-controlled content/instructions. Mitigate with source vetting, content sanitization, and provenance checks.
</details>

<details>
<summary>7. MCP security / least privilege for agents?</summary>

Give tools the minimum permissions needed, scope credentials, require approval for dangerous actions, and validate tool inputs/outputs — so a compromised prompt can't do much damage.
</details>

<details>
<summary>8. Adversarial examples and model extraction?</summary>

Adversarial examples are crafted inputs that fool a model. Model extraction steals a model's behavior by querying it heavily. Defend with rate limits, monitoring, and output constraints.
</details>

## Red Teaming

<details>
<summary>9. Red-team methodology for AI systems?</summary>

Systematically attack your own system (injection, jailbreaks, exfiltration, harmful content), document what works, implement defenses, and re-test — ideally continuously and automated.
</details>

<details>
<summary>10. Automated red teaming?</summary>

Use a model to generate and mutate attacks at scale against your system, scoring which succeed — far broader coverage than manual testing.
</details>

<details>
<summary>11. Test cases: role confusion, instruction override, context manipulation?</summary>

Role confusion: make the model adopt an unsafe persona. Instruction override: "ignore previous instructions." Context manipulation: poison history/retrieved content to steer behavior.
</details>

<details>
<summary>12. Responsible disclosure in AI?</summary>

Report discovered vulnerabilities privately to the owner, allow time to fix before publicizing, and follow a coordinated-disclosure process.
</details>

## Distributed Systems

<details>
<summary>13. Explain the CAP theorem.</summary>

In a distributed system during a network partition, you can guarantee only two of Consistency, Availability, Partition-tolerance — and since partitions happen, you effectively choose C or A. Partitions are not optional.
</details>

<details>
<summary>14. Consistency models?</summary>

Strong (always latest value), eventual (converges over time), causal (respects cause→effect order), read-your-writes (you see your own updates). Stronger = simpler reasoning but higher latency/lower availability.
</details>

<details>
<summary>15. Explain the Raft consensus algorithm.</summary>

Nodes elect a leader; the leader appends client commands to a replicated log and commits an entry once a majority acknowledges it; followers apply committed entries. It provides consistency and tolerates minority failures with clear leader election and safety rules.
</details>

<details>
<summary>16. Replication: master-replica vs multi-master?</summary>

Master-replica: one writer, many read replicas (simple, read-scalable). Multi-master: multiple writers (write-scalable, high availability) but needs conflict resolution.
</details>

<details>
<summary>17. Sharding: consistent hashing and hot spots?</summary>

Consistent hashing maps keys to nodes so adding/removing nodes moves few keys. Hot spots occur when some shards get disproportionate traffic; mitigate with better keys/salting or splitting.
</details>

<details>
<summary>18. What is the Saga pattern?</summary>

For distributed transactions across services: a sequence of local transactions each with a compensating action to undo prior steps if one fails — achieving eventual consistency without a global lock.
</details>

<details>
<summary>19. Why is idempotency essential?</summary>

Networks retry, so operations may run twice; idempotent operations (via idempotency keys) produce the same result without duplicate side effects (double charges, duplicate rows).
</details>

<details>
<summary>20. Kafka vs RabbitMQ vs SQS?</summary>

Kafka: durable, high-throughput, replayable log with ordering per partition. RabbitMQ: flexible routing, low latency. SQS: fully managed, simple, at-least-once. Choose by durability/throughput/ops.
</details>

## Case Studies / System Design

<details>
<summary>21. OpenAI serving; Stripe idempotency keys?</summary>

Large-scale serving uses load balancing, batching, autoscaling, and caching. Stripe attaches an idempotency key to each request so retries don't double-charge — the server dedupes by key.
</details>

<details>
<summary>22. Design serving infra for 1M requests/day.</summary>

~12 rps average; size for peak. Load balancer → stateless API servers → inference workers (batched, autoscaled GPUs) → cache (Redis) → DB. Add rate limiting, queues for spikes, observability, and fallbacks.
</details>

<details>
<summary>23. Design an AI system for 10M daily requests.</summary>

Horizontal scaling behind a CDN/load balancer, caching (semantic + response), request queue with autoscaling GPU workers, sharded/replicated data stores, multi-region, and full monitoring with graceful degradation.
</details>

## Failure / Judgment

<details>
<summary>24. A prompt-injection exploit compromised production — how?</summary>

Untrusted retrieved/tool content carried instructions the agent followed, exfiltrating data or misusing tools. Root cause: mixing data and instructions + over-privileged tools. Fix with isolation, guards, and least privilege.
</details>

<details>
<summary>25. "Canary or blue-green for this deployment?"</summary>

**What I'd do:** canary for gradual, low-risk validation with real traffic; blue-green when you need an instant full switch with fast rollback.
**Why:** canary limits blast radius; blue-green minimizes downtime.
**Tradeoff:** canary is slower to fully roll out; blue-green doubles infrastructure.
**Rejected:** blue-green when you want to catch issues on a small slice first.
</details>
