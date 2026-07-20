# Week 14 — Interview Questions: Evaluation & Reliability Engineering

> Click a question (▸) to expand the answer.

---

## Evaluation Systems

<details>
<summary>1. Why is evaluation the hardest part of AI engineering?</summary>

LLM output is open-ended and non-deterministic with no single correct answer, so you can't just check accuracy. You must define quality, build golden data, handle subjectivity, and catch regressions — an ongoing effort.
</details>

<details>
<summary>2. What makes a good golden dataset?</summary>

Representative of real traffic, covering common and edge cases and hard slices, with agreed-upon correct answers/rubrics, versioned, and large enough to be statistically meaningful.
</details>

<details>
<summary>3. What is slice testing; why?</summary>

Evaluating on subsets (topic, difficulty, demographic, language) instead of just an average — a model can look fine overall while failing badly on an important slice.
</details>

<details>
<summary>4. What is LLM-as-judge; failure modes/biases?</summary>

Using a strong model to score outputs against a rubric. Biases: position/verbosity/self-preference bias, inconsistency, and being fooled by confident-but-wrong answers. Mitigate with clear rubrics, pairwise comparison, and calibration against humans.
</details>

<details>
<summary>5. Reference-based vs reference-free evaluation?</summary>

Reference-based compares to a gold answer (BLEU/ROUGE/exact match). Reference-free judges quality directly (LLM-as-judge, groundedness) — needed when no single gold answer exists.
</details>

<details>
<summary>6. BLEU, ROUGE, BERTScore limitations?</summary>

BLEU/ROUGE measure n-gram overlap (miss meaning/paraphrase); BERTScore uses embeddings (better but still imperfect). None capture factuality or reasoning quality well.
</details>

<details>
<summary>7. Regression testing; eval-driven development?</summary>

Keep a suite of known-good cases and run it on every change so you never silently break what worked. Eval-driven development writes evals before features, like TDD for AI.
</details>

## Observability

<details>
<summary>8. Why aren't traditional logs enough for agentic apps?</summary>

A single request fans out into many model/tool/retrieval steps; you need tracing that links them into one trace with inputs, outputs, latency, cost, and decisions — not flat logs.
</details>

<details>
<summary>9. What should you log for an LLM app (vs a normal API)?</summary>

Prompt, response, model+version, token counts, cost, latency, temperature, retrieved docs, tool calls, and eval scores — because behavior depends on all of these and is non-deterministic.
</details>

<details>
<summary>10. Distributed tracing across an agent pipeline?</summary>

Assign a trace id and span per step (model call, tool, retrieval) so you can see the full tree, timings, and where a multi-step request failed.
</details>

## Cost Tracking

<details>
<summary>11. Break down per-query cost?</summary>

Sum embedding cost + retrieval cost + generation cost (input+output tokens × price), attributed per request/feature/user to find expensive paths.
</details>

<details>
<summary>12. Cost anomaly detection and budget guardrails?</summary>

Alert on sudden spikes (retry loops, huge contexts, prompt injection), and enforce per-user/tenant token budgets and hard caps to prevent runaway spend.
</details>

<details>
<summary>13. What is the cost-vs-quality Pareto frontier?</summary>

The set of configurations where you can't improve quality without raising cost (or vice versa). You pick a point on it based on product needs (e.g., cheaper model for easy queries, big model for hard ones).
</details>

## Deployment Strategies

<details>
<summary>14. Implement canary deployment for an LLM feature?</summary>

Route a small % of traffic to the new model/prompt, compare quality/cost/latency and error rates against the baseline, and gradually ramp up if healthy — roll back automatically on regression.
</details>

<details>
<summary>15. Shadow vs canary vs blue-green?</summary>

Shadow: run the new version in parallel without serving its output (safe comparison). Canary: serve to a small slice. Blue-green: full switch between two identical environments with instant rollback.
</details>

<details>
<summary>16. A/B test LLM responses with significance?</summary>

Randomly assign users to variants, define a metric (thumbs-up, task success), collect enough samples, and use a statistical test to confirm the difference isn't noise.
</details>

## Guardrails & Reliability

<details>
<summary>17. Detect prompt injection at the input layer?</summary>

Classifiers/heuristics for instruction-override patterns, separating system vs user/content, scanning retrieved/tool content (indirect injection), and constraining tool permissions.
</details>

<details>
<summary>18. Output validation; Llama Guard / ShieldGemma?</summary>

Validate format/length/content, run toxicity/safety classifiers, and use guard models (Llama Guard, ShieldGemma) to flag unsafe output before it reaches the user.
</details>

<details>
<summary>19. What is the OWASP Top 10 for LLM Applications?</summary>

A standard list of LLM-specific risks — prompt injection, insecure output handling, training-data poisoning, model DoS, supply chain, sensitive info disclosure, excessive agency, etc. — used as a security checklist.
</details>

<details>
<summary>20. Reliability patterns?</summary>

Retries with exponential backoff, circuit breakers (stop calling a failing dependency), timeouts, graceful fallbacks (degraded but working), and defined SLAs/SLOs.
</details>

## System Design

<details>
<summary>21. Design an evaluation system for a multi-agent coding assistant.</summary>

Golden tasks with expected diffs/tests, automated code-execution checks, LLM-as-judge for quality, per-step tracing, regression suite on every change, canary rollout, and dashboards for quality/cost/latency with alerts.
</details>

## Failure / Judgment

<details>
<summary>22. Prompt injection hit a production RAG system — how, and prevention?</summary>

Malicious instructions embedded in a retrieved document overrode the system prompt (indirect injection). Prevent by isolating/escaping retrieved content, least-privilege tools, input/output guards, and not letting content issue commands.
</details>

<details>
<summary>23. "LLM-as-judge or golden-dataset eval?"</summary>

**What I'd do:** golden dataset for stable, objective checks (regression gates); LLM-as-judge for open-ended quality where no gold answer exists.
**Why:** golden sets are reproducible; judges scale to subjective tasks.
**Tradeoff:** judges are biased/costly; golden sets need curation and miss novel cases.
**Rejected:** relying on either alone — combine them.
</details>
