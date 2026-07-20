# Week 24 — Interview Questions: AI Frameworks, Multimodal & Voice Agents

> Sourced from applied-AI / agent-platform screens. Framework fluency + memory internals.

---

## Orchestration Frameworks
1. **When would you use LangGraph vs raw orchestration? What does the state model buy you?**
2. LangChain vs LlamaIndex vs raw SDKs — when to use each?
3. What does DSPy do differently (programming, not prompting)?
4. CrewAI vs AutoGen vs Haystack vs PydanticAI vs smolagents — where does each fit?
5. When is a visual builder (LangFlow) useful in a real team?

## MCP & Agent SDKs
6. **What is MCP and how does it change tool integration?**
7. Describe MCP architecture: hosts, clients, servers.
8. How do you secure an MCP server (tool permission boundaries, least privilege)?
9. What is the A2A (agent-to-agent) protocol / spinning up subagents?

## Memory Layer
10. **Design short-term and long-term memory for a research agent.**
11. How does thread-id checkpointing work at each super-step?
12. **How do you handle context-window overflow in a long conversation** (trimming vs summarization)?
13. How does long-term memory with namespaces + `store.search()` + k-value retrieval work?
14. What does Mem0 provide? Episodic vs semantic vs procedural memory.

## Graph Memory
15. Why use graph memory for agents? Neo4j/Kuzu + Cypher basics.
16. **Vector memory (Mem0) vs graph memory (Neo4j) — when to use which?**

## Local Deployment & Voice
17. Ollama + OpenWebUI + FastAPI — how do you serve a local model?
18. GGUF vs GPTQ vs AWQ for local models.
19. How does a chained voice agent work (STT → LLM → TTS)? How do you call tools mid-loop?
20. HNSW tuning: `ef_construction`, `max_connections`, MMR — what do they control?

## Failure / Judgment
21. A LangGraph agent lost all memory on restart (in-memory only) — how does checkpointing to Postgres fix it?
