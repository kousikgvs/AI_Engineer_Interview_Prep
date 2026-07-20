# Week 18 — Interview Questions: Capstone, System Design & Hiring Sprint

> Click a question (▸) to expand the answer.

---

## AI-Native System Design (whiteboard drills)

<details>
<summary>1. Design a system like Perplexity (end-to-end).</summary>

Query understanding → web + vector retrieval (hybrid) → rerank → prompt the LLM with sources → stream the answer with inline citations → evaluate faithfulness. Add caching, rate limiting, cost/latency monitoring, and a fallback model.
</details>

<details>
<summary>2. Design a production RAG system.</summary>

Ingest → chunk → embed → hybrid index. Query: rewrite/HyDE → hybrid retrieve → cross-encoder rerank → generate with citations → evaluate groundedness. Add re-indexing on updates, caching, and observability.
</details>

<details>
<summary>3. Design a multi-agent coding assistant; prevent cascading failures.</summary>

Specialized agents + coordinator; isolate failures (timeouts, retries, circuit breakers), validate each output, add a critic, keep humans in the loop for merges, and trace every step. Bound loops to avoid runaway costs.
</details>

<details>
<summary>4. Design an LLM gateway.</summary>

A unified API in front of multiple providers with routing, automatic fallback, semantic + prompt caching, rate limiting, token budgets, retries, cost tracking, and observability — decoupling apps from any single provider.
</details>

<details>
<summary>5. Design an AI system for 10M daily requests.</summary>

CDN/load balancer → stateless API tier → request queue → autoscaled GPU inference workers with batching → caching (semantic/response) → sharded/replicated data stores → multi-region, monitoring, and graceful degradation.
</details>

<details>
<summary>6. What is an ADR; what goes in it?</summary>

An Architecture Decision Record documents a significant decision: context, the options considered, the decision, and its consequences/tradeoffs — so future engineers know why.
</details>

<details>
<summary>7. What's unique about AI system design vs traditional?</summary>

Non-determinism, evaluation difficulty, token cost/latency economics, prompt/model versioning, guardrails/safety, hallucination handling, and rapid model change — beyond normal availability/scalability concerns.
</details>

## Product Engineering

<details>
<summary>8. Customer discovery; Jobs-to-Be-Done?</summary>

Interview users to find the real problem/"job" they're hiring your product to do — focusing on outcomes, not features, so you build what people actually need.
</details>

<details>
<summary>9. Product metrics; which matter for an AI product?</summary>

Activation (first value), retention (do they come back), engagement, and NPS — plus AI-specific ones like task success rate, acceptance rate, and cost per successful task.
</details>

<details>
<summary>10. Unit economics: cost per query and margin?</summary>

Cost per query = tokens × price + retrieval/infra. Margin = price − cost. You must ensure per-query cost stays below what the user/value supports at scale.
</details>

<details>
<summary>11. ICE vs RICE prioritization?</summary>

ICE scores Impact × Confidence × Ease. RICE adds Reach and divides by Effort. Both rank features objectively to focus on highest-value work.
</details>

<details>
<summary>12. A/B test an AI feature with significance?</summary>

Randomize users into variants, pick a metric (task success, retention), gather enough samples, and use a statistical test to confirm the effect is real, not noise.
</details>

## AI Product Intuition

<details>
<summary>13. Why did Cursor / Perplexity / Claude Code win?</summary>

Cursor: AI-native UX, low latency, deep editor integration. Perplexity: fast answers with trusted citations. Claude Code: safe, ergonomic, capable tool use. Common theme: product/UX and trust, not just model quality.
</details>

## Distribution & Hiring

<details>
<summary>14. Why is OSS contribution a hiring signal?</summary>

It's public proof of real, reviewed work in a codebase others depend on — demonstrating skill, collaboration, and initiative better than a résumé line.
</details>

<details>
<summary>15. Write a technical blog post that shows depth?</summary>

Pick a real problem, show your reasoning/tradeoffs, include working code and results, and explain what failed — depth and honesty over hype.
</details>

<details>
<summary>16. What makes a demo sell?</summary>

A clear problem, a fast "wow" moment, a real (not staged) working flow, and a concise story: problem → solution → result. Practice for reliability.
</details>

## Capstone Deliverables

<details>
<summary>17. Walk me through your capstone system design.</summary>

Present the problem, architecture (components, data flow, model choices), key tradeoffs (ADRs), how you evaluated it, what failed, and the live deployment — narrate decisions and why.
</details>

<details>
<summary>18. How did you evaluate your system?</summary>

Golden dataset + LLM-as-judge, task-specific metrics, regression tests, and production monitoring (quality, cost, latency) — show numbers and how you caught regressions.
</details>

<details>
<summary>19. What failed and what would you do differently?</summary>

Give an honest postmortem: what broke, why, what monitoring should have caught it, and the concrete fix — demonstrating engineering maturity.
</details>

<details>
<summary>20. Show me a production deployment.</summary>

A live URL (not localhost) with real infrastructure, monitoring/cost dashboards, and CI/CD — proving you can ship and operate, not just prototype.
</details>

## Judgment

<details>
<summary>21. "Build vs buy — host your own model or use the API?"</summary>

**What I'd do:** start with an API for speed/quality/low ops; self-host at scale for cost, privacy, or control.
**Why:** APIs let you ship and validate fast.
**Tradeoff:** APIs cost more per token and offer less control; self-hosting adds ops burden.
**Rejected:** self-hosting early before volume/privacy justifies it.
</details>
