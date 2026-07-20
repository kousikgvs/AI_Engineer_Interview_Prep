# Week 25 — Interview Questions: AI Ethics, Responsible AI & Generative Foundations

> Sourced from Anthropic-style + enterprise-AI screens. Reasoning and clarity matter more than memorized facts.

---

## Generative Modeling — First Principles
1. **What actually makes a model "generative" vs "discriminative"?** (P(x) vs P(y|x))
2. What are latent variables and what does "sampling from a learned distribution" mean?
3. Map the generative model family: Autoencoders → VAEs → GANs → Diffusion → Autoregressive transformers.
4. Why is next-token prediction a generative objective?

## Bias & Fairness
5. **Where does bias enter an ML system** (data, labeling, objective, deployment)?
6. How do you measure bias? What are common fairness metrics and their tradeoffs?
7. Representational vs allocational harms.
8. How does bias get amplified in generative outputs, and how do you mitigate it?

## Hallucination
9. **Why do LLMs hallucinate, and how would you reduce it in production?**
10. Factual vs faithfulness vs reasoning hallucinations.
11. How do you detect hallucination? Why can't it be fully eliminated?

## Copyright, Provenance & IP
12. **What are the copyright and data-provenance risks of training on scraped data?**
13. What is training-data memorization/regurgitation?
14. Who owns AI-generated content? Why is attribution a trust mechanism?

## Responsible AI & Governance
15. What are the core responsible-AI principles (fairness, accountability, transparency, safety, privacy)?
16. What is a model card and a datasheet for a dataset?
17. How do you handle PII? What is differential privacy (concept level)?
18. What do the EU AI Act and NIST AI RMF cover at a high level?
19. **How would you design responsible-AI guardrails for an enterprise deployment?**

## Judgment
20. "Ship now with a documented hallucination risk, or delay to add grounding + guardrails?" — defend it.
