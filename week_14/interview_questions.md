# Week 14 — Interview Questions: Evaluation & Reliability Engineering

> Sourced from senior AI-engineer / LLMOps screens. "Anyone can build; few can make it reliable."

---

## Evaluation Systems
1. **Why is evaluation the hardest part of AI engineering?**
2. What makes a good golden dataset? How do you construct one?
3. What is slice testing and why evaluate on demographic/topic/difficulty slices?
4. **What is LLM-as-judge? What are its failure modes and biases?**
5. Reference-based vs reference-free evaluation.
6. What are BLEU, ROUGE, BERTScore and their limitations?
7. What is regression testing for LLMs? What is eval-driven development?

## Observability
8. Why aren't traditional logs enough for agentic apps?
9. What should you log for an LLM app that you wouldn't for a normal API?
10. What is distributed tracing across a multi-step agent pipeline?

## Cost Tracking
11. How do you break down per-query cost (embedding, retrieval, generation)?
12. How do you detect a cost anomaly and set budget guardrails?
13. What is the cost-vs-quality Pareto frontier?

## Deployment Strategies
14. **How would you implement canary deployment for an LLM feature?**
15. Shadow mode vs canary vs blue-green — when to use each?
16. How do you A/B test LLM responses with statistical significance?

## Guardrails & Reliability
17. How do you detect prompt injection at the input layer?
18. Output validation: format, length, content, toxicity. What is Llama Guard / ShieldGemma?
19. What is the OWASP Top 10 for LLM Applications?
20. Reliability patterns: retries with backoff, circuit breakers, timeouts, fallbacks, SLAs.

## System Design
21. **Design an evaluation system for a multi-agent coding assistant.**

## Failure / Judgment
22. A prompt-injection attack hit a production RAG system — how did it work, how do you prevent it?
23. "LLM-as-judge or golden-dataset eval?" — defend it.
