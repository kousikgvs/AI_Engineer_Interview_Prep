# Week 11 — Interview Questions: LLM Engineering & Context Engineering

> Click a question (▸) to expand the answer.

---

## Prompt Engineering

<details>
<summary>1. Zero-shot vs few-shot vs many-shot?</summary>

Zero-shot: just instructions. Few-shot: a handful of examples to demonstrate format/behavior. Many-shot: many examples (large context). More examples help harder/format-sensitive tasks at higher token cost.
</details>

<details>
<summary>2. Why does chain-of-thought work?</summary>

Asking the model to reason step-by-step lets it allocate more compute across intermediate steps and condition each step on prior ones, improving multi-step/arithmetic reasoning — it's about the process, not just the answer.
</details>

<details>
<summary>3. What is self-consistency?</summary>

Sample multiple chain-of-thought paths and take the majority answer — averaging out reasoning errors for higher accuracy.
</details>

<details>
<summary>4. Tree-of-thought vs chain-of-thought?</summary>

CoT is a single linear reasoning chain; ToT explores a branching tree of partial solutions with evaluation/backtracking — better for search/planning problems.
</details>

<details>
<summary>5. What is prompt chaining?</summary>

Break a complex task into smaller prompts whose outputs feed the next step — more reliable and debuggable than one giant prompt.
</details>

<details>
<summary>6. What is prompt compression; why matter?</summary>

Getting the same output with fewer tokens (concise instructions, trimming context) to cut cost and latency and fit the context window.
</details>

<details>
<summary>7. How do you design a robust production system prompt?</summary>

Clear role, explicit instructions and output format, constraints/guardrails, few-shot examples, and negative instructions ("don't do X") — tested against edge cases and versioned.
</details>

## Context Engineering

<details>
<summary>8. What goes in the context window and in what order?</summary>

System prompt, tools, retrieved docs, memory, and history. Put the most important content at the start/end (models under-attend to the middle) and drop low-value tokens.
</details>

<details>
<summary>9. Lost-in-the-middle problem; mitigation?</summary>

Models retrieve information at the start/end of long contexts better than the middle. Mitigate by reranking most-relevant content to the edges, compressing, or reducing context size.
</details>

<details>
<summary>10. Long-context strategies?</summary>

Summarize old turns, compress/trim low-value tokens, use a rolling window, or offload to external memory (RAG) — balancing recall vs cost.
</details>

<details>
<summary>11. Context window size vs cost?</summary>

Bigger context includes more information but costs more per call and can dilute attention. Include only what's needed; retrieve rather than stuff everything.
</details>

## Structured Outputs & Tools

<details>
<summary>12. How does JSON mode / structured output work?</summary>

The API constrains generation to valid JSON (often against a schema), so outputs parse reliably into typed objects instead of free text.
</details>

<details>
<summary>13. Why use Pydantic/Instructor for outputs?</summary>

They validate and coerce LLM output against a typed schema, catching malformed/missing fields early with clear errors instead of silently breaking downstream code.
</details>

<details>
<summary>14. How does function/tool calling work?</summary>

You describe tools (name, params schema); the model returns a structured call with arguments; your code runs it and injects the result back into the conversation for the model to use.
</details>

<details>
<summary>15. Handling schema validation failures?</summary>

Retry with the error message, ask the model to fix its output, use constrained decoding, or fall back — never trust unvalidated output.
</details>

<details>
<summary>16. What are ChatML / INST / Alpaca templates?</summary>

Formats that structure prompts into roles/turns (system/user/assistant) so the model knows message boundaries; each model family expects its trained template.
</details>

## Memory

<details>
<summary>17. In-context vs external memory?</summary>

In-context: recent history in the prompt (fast, limited by window). External: vector DB / store retrieved as needed (scales, but adds retrieval latency).
</details>

<details>
<summary>18. Episodic vs semantic vs procedural memory?</summary>

Episodic: past interactions/events. Semantic: facts/knowledge. Procedural: skills/instructions/how-to. Different stores for different recall needs.
</details>

<details>
<summary>19. Design memory for a long-running assistant.</summary>

Short-term: rolling window + summarization of old turns. Long-term: embed and store important facts/events, retrieve relevant ones by similarity, and periodically consolidate/summarize to control size.
</details>

<details>
<summary>20. Compress/summarize memory over long conversations?</summary>

Summarize older turns into compact notes, keep recent turns verbatim, and store detailed history externally for retrieval — bounding token cost while preserving key context.
</details>

## Evaluation & Economics

<details>
<summary>21. Evaluate an LLM without ground truth?</summary>

Use LLM-as-judge with rubrics, curated golden sets, pairwise preference comparisons, reference-free metrics, and human spot checks; track regressions over time.
</details>

<details>
<summary>22. Token cost; build a cost dashboard?</summary>

Input and output tokens are priced (output usually costs more). Log tokens/model/latency per request and aggregate cost per query/user/feature to spot anomalies.
</details>

<details>
<summary>23. Latency economics — P50/P90/P99?</summary>

Percentile latencies: P50 typical, P99 worst-case tail. Tail latency drives user-perceived slowness, so you optimize P95/P99, not just the average.
</details>

<details>
<summary>24. Build vs buy — API vs fine-tune vs self-host?</summary>

API for speed/quality/low ops; self-host for cost at scale, privacy, or control; fine-tune for specialized behavior. Decide by volume, latency, privacy, and team capacity.
</details>

## System Design

<details>
<summary>25. Design a Perplexity-style answer engine.</summary>

Query understanding → web/vector retrieval (hybrid) → rerank → prompt the LLM with sources → stream the answer with inline citations → evaluate faithfulness; cache results and track cost/latency.
</details>

## Failure / Judgment

<details>
<summary>26. Chatbot lost context in long conversations — root cause.</summary>

History overflowed the window and old turns were dropped without summarization, or key facts sat in the lost-middle. Fix with summarization, external memory, and context prioritization.
</details>

<details>
<summary>27. "Fine-tune or RAG for this use case?"</summary>

**What I'd do:** RAG when the task needs current/large/changing knowledge; fine-tune for fixed style/format/behavior.
**Why:** RAG updates by changing data, not weights.
**Tradeoff:** RAG adds retrieval latency/complexity; fine-tuning bakes knowledge in but goes stale.
**Rejected:** fine-tuning for frequently-changing facts.
</details>
