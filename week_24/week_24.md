# Week 24 — AI Frameworks, Multimodal & Voice Agents Mastery

**Block:** J — Deep Learning & Multimodal Depth (Advanced Depth Track)
**Goal:** Master the production framework ecosystem end-to-end — the orchestration, memory, protocol, and multimodal layers that turn models into real products.

> Deepens Weeks 11–13 (LLM engineering, RAG, agents) with hands-on framework fluency and adds voice + graph memory.

---

## What You Will Learn

### Orchestration Frameworks
- LangChain — chains, LCEL, loaders, retrievers, tools
- LangGraph — nodes, edges, state, StateGraph; stateful agent workflows
- Checkpointing workflows for persistence (MongoDB/Postgres)
- Debugging stateful agent graphs
- LlamaIndex — data connectors, documents, nodes, indexes, query engines
- When to use LlamaIndex vs LangChain vs raw SDKs
- DSPy — programming (not prompting) LLMs; optimizers and signatures
- Haystack, CrewAI, AutoGen, PydanticAI — where each fits
- Hugging Face smolagents — lightweight code-first agents with `@tool`
- LangFlow — visual workflow building for rapid RAG/agent prototyping

### Model Context Protocol (MCP) & Agent SDKs
- What MCP is and why it matters (standardizing tool access)
- MCP architecture: hosts, clients, servers
- Building a minimal MCP server
- MCP security — tool permission boundaries, least privilege
- Agent SDK concepts: hosted tools, function tools, agent-as-a-tool
- A2A (agent-to-agent) protocol and spinning up subagents

### The Memory Layer (Deep)
- Short-term memory: checkpointing at each super-step via thread-id
- Persisting short-term memory to Postgres (Dockerized)
- Context-window overflow handling: trimming (`trim_messages`, ~500 tokens) and summarization
- Long-term memory: namespaces `(user, u1)`, key-value stores, embedding-backed search
- `store.search()` over stored values, retrieve by k-value
- Mem0 setup and vector-DB-backed agent memory
- Episodic, semantic, and procedural memory
- Deep agents and deep research agents

### Graph Memory & Knowledge Graphs
- Why graph memory matters for agents
- Neo4j and Kuzu for graph storage
- Cypher query basics
- Adding and testing graph memory in an agent
- GraphRAG vs vector memory — when to use which

### Local LLM Deployment
- Ollama as a local runtime; Dockerized LLM environments
- OpenWebUI; FastAPI services on top of Ollama
- Quantized local models: GGUF, GPTQ, AWQ tradeoffs

### Multimodal & Voice Agents
- Multimodal agents and image-based reasoning
- Speech-to-Speech voice agents
- Chained voice agents: STT → LLM → TTS
- Tool calling from within a voice loop
- Vector index tuning for retrieval agents: HNSW (`ef_construction`, `max_connections`), MMR, periodic config reload
- Batch/throughput tuning: batch size, multithreading, pod sizing

---

## Project (pick two)
- **Voice agent:** STT → LLM → TTS chained agent that holds a natural conversation and calls tools
- **AI Interview Taker (Mercor-style):** conducts a voice interview, scores against a Pydantic rubric, generates a hiring report
- **Graph-memory research agent:** LangGraph agent with Neo4j graph memory + Mem0 long-term memory over a knowledge domain
- **MCP-powered coding agent:** build an MCP server exposing repo tools; agent uses it with least-privilege permissions
- **Local RAG stack:** Ollama + OpenWebUI + FastAPI, fully offline, with a quantized GGUF model

---

## Papers / Reading
- Read: "DSPy: Compiling Declarative Language Model Calls into Self-Improving Pipelines" — Khattab et al.
- Read: MCP specification overview (Anthropic)

---

## Failure Friday
- Analyze: A stateful LangGraph agent that lost all memory on restart — no checkpointing, in-memory only. How thread-id checkpointing to Postgres fixes it.

---

## Interview Questions This Week Prepares You For
- "When would you use LangGraph vs raw orchestration? What does the state model buy you?"
- "How do you handle context-window overflow in a long conversation?"
- "What is MCP and how does it change tool integration?"
- "Design short-term and long-term memory for a research agent"

---

## Engineering Judgment Question
**"Vector memory (Mem0) or graph memory (Neo4j) for this agent's long-term memory?"**
Write your answer covering: what you would do, why, what tradeoff you are making, and what alternative you rejected.
