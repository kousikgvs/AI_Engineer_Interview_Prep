# Week 25 — Interview Questions: AI Ethics, Responsible AI & Generative Foundations

> Click a question (▸) to expand the answer.

---

## Generative Modeling — First Principles

<details>
<summary>1. What makes a model "generative" vs "discriminative"?</summary>

A generative model learns the data distribution P(x) (or P(x|context)) so it can produce new samples; a discriminative model learns P(y|x) — the boundary between classes — to label existing data.
</details>

<details>
<summary>2. What are latent variables / sampling from a learned distribution?</summary>

Latent variables are hidden factors that explain the data; generative models learn a distribution over them, and you sample latents then decode to produce new, varied outputs.
</details>

<details>
<summary>3. Map the generative model family.</summary>

Autoencoders (reconstruct) → VAEs (probabilistic latent) → GANs (adversarial) → Diffusion (iterative denoising) → Autoregressive transformers (next-token) — all model the data distribution in different ways.
</details>

<details>
<summary>4. Why is next-token prediction a generative objective?</summary>

Modeling P(next token | previous tokens) factorizes the joint distribution over sequences; sampling from it generates new coherent text — so it's inherently generative.
</details>

## Bias & Fairness

<details>
<summary>5. Where does bias enter an ML system?</summary>

Data (unrepresentative/historical bias), labeling (annotator bias), objective (what you optimize), and deployment context (who uses it, how). Bias can enter at every stage.
</details>

<details>
<summary>6. How do you measure bias; fairness metrics tradeoffs?</summary>

Metrics like demographic parity, equal opportunity, and equalized odds — but they can be mutually incompatible, so you choose based on the harm you most want to avoid; measure across slices.
</details>

<details>
<summary>7. Representational vs allocational harms?</summary>

Representational: reinforcing stereotypes/demeaning groups (e.g., biased text/images). Allocational: unfairly distributing resources/opportunities (e.g., biased hiring/loan decisions).
</details>

<details>
<summary>8. Bias amplification in generative outputs; mitigation?</summary>

Models can amplify majority patterns in training data. Mitigate via balanced/curated data, debiasing, slice-based evaluation, guardrails, and human review.
</details>

## Hallucination

<details>
<summary>9. Why do LLMs hallucinate; reduce in production?</summary>

They generate the most probable continuation, which needn't be factual, and have no ground-truth access. Reduce with RAG grounding, citations, self-consistency, lower temperature, verification, and guardrails.
</details>

<details>
<summary>10. Factual vs faithfulness vs reasoning hallucinations?</summary>

Factual: wrong facts. Faithfulness: contradicts the provided source. Reasoning: invalid logical steps. Each needs different mitigation (grounding, source-checking, verification).
</details>

<details>
<summary>11. Detect hallucination; why can't it be fully eliminated?</summary>

Detect via groundedness checks, citation verification, consistency across samples, and LLM-as-judge. It can't be fully removed because generation is probabilistic and the model lacks perfect knowledge/verification.
</details>

## Copyright, Provenance & IP

<details>
<summary>12. Copyright/provenance risks of training on scraped data?</summary>

Training on copyrighted/unlicensed data raises legal exposure; models can memorize and regurgitate content, and unclear provenance risks license violations and reputational/legal harm.
</details>

<details>
<summary>13. What is memorization/regurgitation?</summary>

The model reproduces near-verbatim training examples (code, text, PII), which can leak copyrighted or private data. Mitigate with dedup, filtering, and output checks.
</details>

<details>
<summary>14. Who owns AI-generated content; why attribution?</summary>

Ownership is legally unsettled and jurisdiction-dependent. Attribution/citation builds trust, lets users verify, and respects sources — increasingly expected for responsible deployment.
</details>

## Responsible AI & Governance

<details>
<summary>15. Core responsible-AI principles?</summary>

Fairness, accountability, transparency, safety, and privacy — designing systems that are unbiased, explainable, secure, and respectful of user data, with clear ownership of outcomes.
</details>

<details>
<summary>16. What is a model card / datasheet for a dataset?</summary>

A model card documents a model's intended use, performance, limitations, and risks. A datasheet documents a dataset's composition, collection, and biases — both improve transparency.
</details>

<details>
<summary>17. Handle PII; differential privacy (concept)?</summary>

Minimize/anonymize PII, restrict access, and encrypt. Differential privacy adds calibrated noise so outputs don't reveal any individual's data while preserving aggregate utility.
</details>

<details>
<summary>18. EU AI Act and NIST AI RMF at a high level?</summary>

The EU AI Act is risk-tiered regulation (banned/high/limited/minimal risk) with obligations by tier. NIST AI RMF is a voluntary framework (Govern, Map, Measure, Manage) for managing AI risk.
</details>

<details>
<summary>19. Design responsible-AI guardrails for an enterprise deployment.</summary>

Input/output validation, PII redaction, toxicity/safety classifiers, prompt-injection defense, human-in-the-loop for high-stakes actions, audit logging, bias/hallucination evals, and clear escalation — layered defense with monitoring.
</details>

## Judgment

<details>
<summary>20. "Ship now with a documented hallucination risk, or delay to add grounding + guardrails?"</summary>

**What I'd do:** for low-stakes uses, ship with clear disclaimers and monitoring; for high-stakes (medical/legal/financial), delay to add grounding and guardrails.
**Why:** risk should match the cost of being wrong.
**Tradeoff:** speed/learning vs user harm and trust.
**Rejected:** shipping unguarded in high-stakes domains, or over-delaying a low-risk feature.
</details>
