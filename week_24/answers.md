# Week 24 — Answers with Code: AI Frameworks, Multimodal & Voice Agents

> Orchestration frameworks, MCP, multimodal, and voice pipelines.

---

## Orchestration Frameworks

### 1. LangGraph / state (detailed)
Agents as a state graph (nodes/edges) with shared state + checkpointing → branching, loops, persistence, resumability. Raw orchestration = build it yourself.

### 2. LangChain vs LlamaIndex vs SDKs (detailed)
LangChain: general chains/agents/tools. LlamaIndex: data-centric RAG. Raw SDKs: max control. Retrieval-heavy → LlamaIndex; orchestration → LangChain; frameworks in the way → SDKs.

### 3. DSPy (detailed)
Program pipelines with declarative modules; an optimizer tunes prompts/weights against a metric — "programming, not prompting."

### 4. CrewAI/AutoGen/Haystack/PydanticAI/smolagents (easy)
Multi-agent (CrewAI/AutoGen), production RAG (Haystack), type-safe agents (PydanticAI), lightweight code-first (smolagents).

### 5. Visual builder (LangFlow) (easy)
Rapid prototyping/demos; production usually moves to code for testing/versioning.

---

## MCP & Agent SDKs

### 6. Model Context Protocol (detailed)
Open standard for connecting models to tools/data via servers → reusable, interoperable tool integrations across clients.
```python
# MCP server exposes tools; any MCP client (IDE, agent) can call them
@server.tool()
def search_docs(query: str) -> list[str]:
    return db.search(query)
```

### 7. Why MCP matters (easy)
Write a tool once as an MCP server → any compatible agent/app uses it (no bespoke glue per client).

### 8. Agent SDK patterns (easy)
Define tools + a loop; SDKs handle tool-call parsing, retries, and tracing.

---

## Multimodal

### 9. How vision-language models work (detailed)
Image encoder (ViT) → project patches into the LLM token space → LLM attends over text + image tokens jointly.

### 10. CLIP (detailed + code)
Contrastive training aligns image and text embeddings in one space → zero-shot classification/retrieval.
```python
img_emb = image_encoder(image); txt_emb = text_encoder(text)
sim = (img_emb @ txt_emb.T) / (norm(img_emb) * norm(txt_emb))  # cosine
```

### 11. Cross-attention fusion (easy)
Text queries attend to image features (or fused tokens) → grounded multimodal reasoning.

### 12. Multimodal RAG (easy)
Embed images + text in a shared space; retrieve across modalities for the query.

---

## Voice Agents

### 13. Voice pipeline (detailed)
STT (speech→text) → LLM → TTS (text→speech). Latency budget across all three; stream to feel real-time.

### 14. Streaming STT (easy)
Transcribe incrementally as audio arrives → lower perceived latency; handle partial/final results.

### 15. Reducing voice latency (detailed)
Stream STT + stream LLM tokens + stream TTS; start TTS on first sentence; use VAD to detect end-of-speech quickly.

### 16. Interruption / barge-in (easy)
Detect user speech during TTS (VAD), stop playback, and re-listen → natural conversation.

---

## Coding

### 17. MCP tool server (see Q6).
### 18. CLIP similarity (see Q10).
### 19. Voice loop:
```python
async def voice_turn(audio):
    text = await stt_stream(audio)
    async for tok in llm_stream(text):
        await tts_stream(tok)          # speak as tokens arrive
```

---

## Judgment / Failure

### 20. Framework fights you: over-abstraction hides control → drop to raw SDK for the hot path.
### 21. Voice agent feels laggy: sequential STT→LLM→TTS → stream all stages, start TTS early, fast VAD.
### 22. Multimodal retrieval misses: separate embedding spaces → use a shared/aligned space (CLIP-style).
