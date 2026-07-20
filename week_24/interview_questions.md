# Week 24 — Interview Questions: AI Frameworks, Multimodal & Voice Agents

> Click a question (▸) to expand the answer.

---

## Orchestration Frameworks

<details>
<summary>1. LangGraph vs raw orchestration — what does state buy you?</summary>

LangGraph models agents as a state graph (nodes/edges) with explicit shared state and checkpointing, so you get branching, loops, persistence, and resumability. Raw orchestration means building all that yourself.
</details>

<details>
<summary>2. LangChain vs LlamaIndex vs raw SDKs?</summary>

LangChain: general chains/agents/tools. LlamaIndex: data-centric RAG (connectors, indexes, query engines). Raw SDKs: maximum control, minimal abstraction. Use LlamaIndex for retrieval-heavy apps, LangChain for orchestration, SDKs when frameworks get in the way.
</details>

<details>
<summary>3. What does DSPy do differently?</summary>

You program LLM pipelines with declarative modules and let an optimizer tune the prompts/weights against a metric — "programming, not prompting," replacing manual prompt tweaking.
</details>

<details>
<summary>4. CrewAI vs AutoGen vs Haystack vs PydanticAI vs smolagents?</summary>

CrewAI/AutoGen: multi-agent collaboration. Haystack: production RAG/search pipelines. PydanticAI: type-safe agent building. smolagents: lightweight code-first agents. Choose by whether you need multi-agent, RAG, type safety, or minimalism.
</details>

<details>
<summary>5. When is a visual builder (LangFlow) useful?</summary>

For rapid prototyping, letting non-experts wire flows, and demoing — but production usually moves to code for testing/version control.
</details>

## MCP & Agent SDKs

<details>
<summary>6. What is MCP and how does it change tool integration?</summary>

The Model Context Protocol is an open standard for connecting models to tools/data via servers, so any MCP-compatible client can use any MCP server — replacing bespoke per-integration glue.
</details>

<details>
<summary>7. MCP architecture: hosts, clients, servers?</summary>

The host (app) runs clients that connect to MCP servers exposing tools/resources/prompts. The model requests tool use; the client calls the server and returns results.
</details>

<details>
<summary>8. Secure an MCP server (least privilege)?</summary>

Expose only necessary tools with scoped permissions, validate inputs, require approval for dangerous actions, authenticate clients, and audit calls — so a compromised prompt can't overreach.
</details>

<details>
<summary>9. What is A2A / spinning up subagents?</summary>

Agent-to-agent protocols let agents delegate to specialized subagents (a manager spawning workers), coordinating multi-agent work with clear interfaces.
</details>

## Memory Layer

<details>
<summary>10. Design short-term and long-term memory for a research agent.</summary>

Short-term: checkpointed conversation state per thread-id with trimming/summarization on overflow. Long-term: embed important findings into a store (namespaced), retrieve by similarity, and periodically consolidate.
</details>

<details>
<summary>11. How does thread-id checkpointing work per super-step?</summary>

After each graph super-step, state is saved keyed by thread-id (in-memory or Postgres), so the agent can resume exactly where it left off after a restart or interruption.
</details>

<details>
<summary>12. Handle context-window overflow (trimming vs summarization)?</summary>

Trimming drops old messages (e.g., keep last ~N tokens); summarization compresses old turns into a running summary. Trim for speed, summarize to preserve important context.
</details>

<details>
<summary>13. Long-term memory: namespaces + store.search + k-value?</summary>

Store facts under a namespace (e.g., per user) as key-value/embedded entries; at query time `store.search()` finds the top-k most relevant items by similarity to inject into context.
</details>

<details>
<summary>14. What does Mem0 provide; memory types?</summary>

Mem0 is a memory layer that extracts, stores, and retrieves salient facts across sessions. Episodic (events), semantic (facts), procedural (skills) memory serve different recall needs.
</details>

## Graph Memory

<details>
<summary>15. Why graph memory; Neo4j/Kuzu + Cypher?</summary>

Graphs store entities and relationships, enabling multi-hop and relational reasoning that vector similarity misses. Neo4j/Kuzu are graph DBs; Cypher is the query language (MATCH patterns over nodes/edges).
</details>

<details>
<summary>16. Vector memory (Mem0) vs graph memory (Neo4j)?</summary>

Vector memory for fuzzy semantic recall of facts/passages; graph memory for structured, connected, multi-hop relationships. Use graphs when relationships between entities matter.
</details>

## Local Deployment & Voice

<details>
<summary>17. Ollama + OpenWebUI + FastAPI — serve a local model?</summary>

Ollama runs a quantized model locally with an API; OpenWebUI gives a chat UI; FastAPI wraps custom logic/endpoints on top — a fully offline stack.
</details>

<details>
<summary>18. GGUF vs GPTQ vs AWQ for local models?</summary>

GGUF: format for CPU/GPU inference via llama.cpp/Ollama. GPTQ/AWQ: 4-bit GPU quantization methods (AWQ protects salient weights). Use GGUF for local/CPU, GPTQ/AWQ for GPU serving.
</details>

<details>
<summary>19. How does a chained voice agent work (STT → LLM → TTS)?</summary>

Speech-to-text transcribes audio; the LLM reasons/acts (with tools); text-to-speech voices the reply. Streaming and low-latency components keep the conversation natural.
</details>

<details>
<summary>20. HNSW tuning: ef_construction, max_connections, MMR?</summary>

`max_connections` (M) sets graph connectivity (recall vs memory); `ef_construction` controls index build quality; higher `ef_search` raises recall at query time. MMR diversifies results to avoid duplicates.
</details>

## Failure / Judgment

<details>
<summary>21. LangGraph agent lost memory on restart — fix?</summary>

It used in-memory state only. Fix by checkpointing state to Postgres keyed by thread-id, so the agent persists and resumes across restarts.
</details>

<details>
<summary>22. "Vector memory (Mem0) or graph memory (Neo4j)?"</summary>

**What I'd do:** vector memory for semantic recall of facts/notes; graph memory when relationships and multi-hop reasoning between entities matter.
**Why:** vectors are simple and fuzzy-match well; graphs capture structure.
**Tradeoff:** graphs add modeling/ops complexity.
**Rejected:** graph memory when simple similarity recall suffices.
</details>
