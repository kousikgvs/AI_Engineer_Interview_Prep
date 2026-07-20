# Week 14 — Evaluation & Reliability Engineering

**Block:** E — Evaluation + Reliability + Security  
**Goal:** Most AI engineers can build. Very few can make it reliable. This is what gets you senior roles.

---

## What You Will Learn

### Evaluation Systems
- Why evaluation is the hardest part of AI engineering
- Golden dataset construction — what makes a good eval set
- Slice testing — evaluating on demographic, topic, and difficulty slices
- LLM-as-judge — design, calibration, and bias sources
- Reference-based vs reference-free evaluation
- BLEU, ROUGE, BERTScore — and their limitations
- Regression testing — never break what already works
- Eval-driven development — write evals before you build features

### Observability
- Tracing — LangFuse, Braintrust, custom solutions
- What to log: inputs, outputs, latency, cost, model version, retrieval docs
- Structured logging for AI systems
- Distributed tracing for multi-step agent pipelines

### Cost Tracking
- Per-query cost breakdown: embedding, retrieval, generation
- Cost anomaly detection
- Budget guardrails
- Cost vs quality Pareto frontier

### Deployment Strategies
- Canary deployments — send a small percentage of traffic to the new model
- Shadow mode — run new model in parallel without serving its output
- A/B testing LLM responses — statistical significance
- Feature flags for AI behavior
- Blue-green deployments

### Guardrails
- Input validation — prompt injection detection
- Output validation — format, length, content checks
- Topical guardrails — keeping the model focused on its task
- Toxicity and safety classifiers
- Moderation APIs (OpenAI Moderation and similar)
- Structured output validation with Pydantic
- Guardrails AI, NeMo Guardrails, Llama Guard, ShieldGemma (concept level)
- OWASP Top 10 for LLM Applications — the standard threat checklist

### Reliability Patterns
- Retry logic with exponential backoff
- Circuit breakers
- Timeout management
- Fallback strategies — degraded but still functional
- SLA definition for AI systems

---

## Project
Productionize the Week 13 agent with:
- Full eval harness with a golden dataset
- LLM-as-judge for response quality scoring
- Canary deployment with traffic splitting
- Cost dashboard per request
- Tracing with LangFuse
- Guardrails for prompt injection and off-topic detection
- Alerting on quality regression

---

## Paper to Read
- "Chatbot Arena: An Open Platform for Evaluating LLMs by Human Preference" — Zheng et al.

---

## Failure Friday
- Analyze: A prompt injection attack on a production RAG system — how it worked and how to prevent it

---

## Interview Questions This Week Prepares You For

<details>
<summary>"Design an evaluation system for a multi-agent coding assistant"</summary>

Golden tasks with expected diffs/tests, automated code-execution checks, LLM-as-judge for quality, per-step tracing, a regression suite on every change, canary rollout, and dashboards for quality/cost/latency with alerts.
</details>

<details>
<summary>"How would you implement canary deployment for an LLM-based feature?"</summary>

Route a small % of traffic to the new model/prompt, compare quality/cost/latency/errors against the baseline, ramp up if healthy, and auto-roll-back on regression.
</details>

<details>
<summary>"What is LLM-as-judge? What are its failure modes?"</summary>

Using a strong model to score outputs against a rubric. Failure modes: position/verbosity/self-preference bias, inconsistency, and being fooled by confident wrong answers. Mitigate with clear rubrics, pairwise comparison, and human calibration.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"LLM-as-judge or golden dataset eval?"</strong></summary>

**What I'd do:** golden dataset for stable objective regression gates; LLM-as-judge for open-ended quality with no gold answer.
**Why:** golden sets are reproducible; judges scale to subjective tasks.
**Tradeoff:** judges are biased/costly; golden sets need curation.
**Rejected:** relying on either alone — combine them.
</details>
