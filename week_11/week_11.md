# Week 11 — LLM Engineering & Context Engineering

**Block:** D — LLM Engineering + RAG + Agents  
**Goal:** Move from training models to building products with them.

---

## What You Will Learn

### Prompt Engineering
- Zero-shot, few-shot, many-shot prompting
- Chain-of-thought (CoT) prompting — why it works (process vs answer)
- Tree-of-thought (ToT)
- Self-consistency — majority voting across multiple CoT paths
- Role prompting and system prompt design
- Prompt chaining — breaking complex tasks into smaller steps
- Prompt compression — same output, fewer tokens
- Negative prompting — telling the model what NOT to do

### Context Engineering
- The context window as the model's entire working environment
- What to include: tools, memory, retrieved docs, conversation history
- Context prioritization — what you put first matters
- Lost-in-the-middle problem — models do worse on info buried in the middle
- Long context strategies: summarization, compression, rolling window
- System prompt design for production systems
- Context window size vs cost tradeoffs

### Structured Outputs
- JSON mode and structured output APIs
- Chat templating and prompt serialization formats: ChatML, INST, Alpaca
- Pydantic integration with LLM outputs
- Function calling — how it works at the API level
- Tool calling — definitions, arguments, result injection
- Instructor library patterns
- Handling schema validation failures gracefully

### Memory Systems
- In-context memory — conversation history
- External memory — vector DB retrieval
- Episodic memory — storing past interactions
- Semantic memory — storing facts and knowledge
- Procedural memory — storing skills and instructions
- Memory compression and summarization

> Deep dive: framework-level memory (Mem0, graph memory with Neo4j/Kuzu, LangGraph checkpointing, context-overflow trimming/summarization) is covered in the Advanced Depth Track — [Week 24](../week_24/week_24.md).

### Evaluation (First Look)
- Why "vibes" are not enough
- Golden datasets — building ground truth
- LLM-as-judge — using a model to evaluate a model
- Unit tests for LLM behavior
- Regression testing — don't break what works

### Economics of AI (First Look)
- Token costs — input vs output pricing
- Latency economics — P50/P90/P99 and user experience
- GPU economics — spot vs on-demand vs reserved capacity
- Open-source vs closed-source tradeoffs at scale
- Build vs buy — API vs fine-tune vs self-host

---

## Project
- Build a Perplexity clone with streaming, citations, and source attribution
- Implement an LLM-as-judge evaluation system
- Build a memory system that persists across conversations
- Cost dashboard tracking per-query token spend

---

## Paper to Read
- "Chain-of-Thought Prompting Elicits Reasoning in Large Language Models" — Wei et al.

---

## Failure Friday
- Analyze: A production chatbot that lost context in long conversations — root cause in context management

---

## Interview Questions This Week Prepares You For

<details>
<summary>"How would you design a memory system for a long-running AI assistant?"</summary>

Short-term: rolling window + summarize old turns. Long-term: embed and store important facts/events, retrieve relevant ones by similarity, and periodically consolidate/summarize to bound size.
</details>

<details>
<summary>"What is the lost-in-the-middle problem and how do you mitigate it?"</summary>

Models attend better to the start/end of long contexts than the middle. Mitigate by reranking key content to the edges, compressing, or shrinking the context.
</details>

<details>
<summary>"How do you evaluate an LLM without a ground truth dataset?"</summary>

LLM-as-judge with rubrics, curated golden sets, pairwise preference comparisons, reference-free metrics, and human spot checks — tracking regressions over time.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"Fine-tune or use RAG for this use case?"</strong></summary>

**What I'd do:** RAG for current/large/changing knowledge; fine-tune for fixed style/format/behavior.
**Why:** RAG updates by changing data, not weights.
**Tradeoff:** RAG adds retrieval latency; fine-tuning bakes knowledge in but goes stale.
**Rejected:** fine-tuning for frequently-changing facts.
</details>
