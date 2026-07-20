# Week 11 — Interview Questions: LLM Engineering & Context Engineering

> Sourced from applied-AI / AI-product engineer screens (Perplexity, Harvey, Glean style).

---

## Prompt Engineering
1. Zero-shot vs few-shot vs many-shot — when does each help?
2. **Why does chain-of-thought work?** (process vs answer)
3. What is self-consistency and how does majority voting improve accuracy?
4. Tree-of-thought vs chain-of-thought.
5. What is prompt chaining and when do you break a task into steps?
6. What is prompt compression and why does it matter for cost?
7. How do you design a robust production system prompt?

## Context Engineering
8. What goes into a context window and in what priority order?
9. **What is the lost-in-the-middle problem and how do you mitigate it?**
10. Long-context strategies: summarization, compression, rolling window — tradeoffs.
11. Context window size vs cost — how do you reason about it?

## Structured Outputs & Tools
12. How does JSON mode / structured output work at the API level?
13. Why use Pydantic (or Instructor) to validate LLM outputs?
14. **How does function/tool calling work?** Definitions → arguments → result injection.
15. How do you handle schema validation failures gracefully?
16. What are ChatML / INST / Alpaca chat templates?

## Memory
17. In-context vs external memory — when do you use each?
18. Episodic vs semantic vs procedural memory.
19. **How would you design a memory system for a long-running AI assistant?**
20. How do you compress/summarize memory over long conversations?

## Evaluation & Economics
21. **How do you evaluate an LLM without a ground-truth dataset?** (LLM-as-judge, golden sets)
22. Token cost: input vs output pricing. How do you build a cost dashboard?
23. Latency economics — what are P50/P90/P99 and why do they matter?
24. Build vs buy — API vs fine-tune vs self-host?

## System Design
25. **Design a Perplexity-style answer engine** with streaming, citations, source attribution.

## Failure / Judgment
26. A chatbot lost context in long conversations — root-cause in context management.
27. "Fine-tune or RAG for this use case?" — defend it.
