# Week 25 — AI Ethics, Responsible AI & Generative Modeling Foundations

**Block:** K — Responsible & Generative AI (Advanced Depth Track)
**Goal:** Understand what makes a model *generative* from first principles, and build the ethical and responsible-AI judgment that every serious GenAI role demands. Anthropic and enterprise buyers over-index on this.

> Why this week exists: The core sprint teaches how to build and ship. This week covers the *should* — bias, hallucination, copyright, and responsible deployment — plus a first-principles framing of generative modeling that ties the whole program together. Sourced from the Vizuara Minor in GenAI (Course 1: Foundations + Ethics).

---

## What You Will Learn

### Generative Modeling — First Principles
- What actually makes a model "generative" vs "discriminative"
- Probabilistic intuition: modeling `P(x)` and `P(x | context)` instead of just `P(y | x)`
- Latent variables and sampling from a learned distribution
- The generative model family map: Autoencoders → VAEs → GANs → Diffusion → Autoregressive Transformers
- Why autoregressive next-token prediction is a generative objective
- How the same probabilistic core spans text, images, and creative AI

### Applications of Generative AI
- Text generation, summarization, translation, code
- Image generation and editing; creative AI (design, music, video)
- Industry use cases: healthcare, legal, finance, education, media
- Where generative AI creates real value vs hype

### AI Ethics & Bias
- Where bias comes from: data, labeling, objective, deployment context
- Measuring bias — fairness metrics and their tradeoffs
- Representational vs allocational harms
- Bias amplification in generative outputs
- Mitigation: data curation, debiasing, evaluation on slices (ties to Week 14 slice testing)

### Hallucination — Causes & Mitigation
- Why LLMs hallucinate (probabilistic generation, no ground truth, training gaps)
- Types of hallucination: factual, faithfulness, reasoning
- Detection strategies and confidence/calibration signals
- Mitigation: grounding with RAG, citations, self-consistency, guardrails, human-in-the-loop
- The limits of mitigation — why hallucination cannot be fully eliminated

### Copyright, Data Provenance & IP
- Training data copyright — the open legal questions
- Memorization and regurgitation of training data
- Data provenance and licensing (what you can and cannot train on)
- Output ownership — who owns AI-generated content
- Attribution and citation as trust mechanisms

### Responsible AI & Governance
- Responsible AI principles: fairness, accountability, transparency, safety, privacy
- Model cards and datasheets for datasets
- Privacy: PII handling, differential privacy (concept level), data minimization
- Regulatory landscape (concept level): EU AI Act, NIST AI RMF
- Alignment understanding — why it matters for deployment (ties to Week 16)
- Responsible disclosure and red-teaming for harm (ties to Week 15)
- Human oversight, escalation, and the right to explanation

---

## Project
- Write a **Model Card + Ethics Statement** for one of your earlier capstone/agent builds: intended use, limitations, bias analysis, hallucination risks, data provenance, and mitigations
- Build a **hallucination eval harness**: run your RAG system on adversarial factual queries, measure hallucination rate, and add grounding + guardrails to reduce it — report before/after
- Run a **bias slice test** on a classifier or LLM output across demographic/topic slices and document disparities

---

## Papers / Reading
- Read: "On the Dangers of Stochastic Parrots" — Bender, Gebru et al.
- Read: "Model Cards for Model Reporting" — Mitchell et al.
- Read: "Survey of Hallucination in Natural Language Generation" — Ji et al. (skim)
- Reference: NIST AI Risk Management Framework (overview)

---

## Failure Friday
- Analyze: A production GenAI product that generated defamatory or copyrighted content — how it happened, the legal/reputational blast radius, and what guardrails + provenance controls would have prevented it.

---

## Interview Questions This Week Prepares You For

<details>
<summary>"What actually makes a model generative? Explain from a probabilistic standpoint"</summary>

A generative model learns the data distribution P(x) (or P(x|context)) so it can sample new data; a discriminative model only learns P(y|x), the decision boundary. Generation = sampling from the learned distribution.
</details>

<details>
<summary>"Why do LLMs hallucinate, and how would you reduce it in a production system?"</summary>

They output the most probable continuation, which needn't be factual, with no ground-truth access. Reduce with RAG grounding, citations, self-consistency, lower temperature, verification, and guardrails.
</details>

<details>
<summary>"Where does bias enter an ML system, and how do you measure and mitigate it?"</summary>

Data, labeling, objective, and deployment context. Measure with fairness metrics across slices (demographic parity, equal opportunity); mitigate via balanced data, debiasing, evaluation, and human review.
</details>

<details>
<summary>"What are the copyright and data-provenance risks of training on scraped data?"</summary>

Legal exposure from copyrighted/unlicensed content, memorization/regurgitation of training data (including PII), and license violations from unclear provenance — mitigate with dedup, filtering, and provenance tracking.
</details>

<details>
<summary>"How would you design responsible-AI guardrails for an enterprise deployment?"</summary>

Layered: input/output validation, PII redaction, toxicity/safety classifiers, prompt-injection defense, human-in-the-loop for high-stakes actions, audit logging, bias/hallucination evals, and clear escalation — with monitoring.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"Ship now with a documented hallucination risk, or delay to add grounding + guardrails?"</strong></summary>

**What I'd do:** low-stakes — ship with disclaimers and monitoring; high-stakes (medical/legal/financial) — delay to add grounding and guardrails.
**Why:** risk should match the cost of being wrong.
**Tradeoff:** speed/learning vs user harm and trust.
**Rejected:** shipping unguarded in high-stakes domains, or over-delaying a low-risk feature.
</details>
