# Week 13 — Answers with Code: Agent Engineering

> Agent loops, tool calling, memory, and multi-agent patterns with code.

---

## Agent Foundations

### 1. Agent / PRAOR loop (detailed + code)
Perceive → Reason → Act → Observe → Repeat toward a goal.
```python
def agent(goal, tools, max_steps=10):
    history = [f"Goal: {goal}"]
    for _ in range(max_steps):
        thought = llm(history)                 # Reason
        if "FINAL:" in thought:
            return thought.split("FINAL:")[1]
        call = parse_tool(thought)             # Act
        obs = tools[call.name](**call.args)    # Observe
        history.append(f"{thought}\nObservation: {obs}")
    return "max steps reached"
```

### 2. Failure at each stage (detailed)
Perceive: bad context. Reason: flawed plan. Act: wrong tool/args. Observe: misread result. Repeat: compounding errors.

### 3. Environment types (easy)
Observable vs partial, deterministic vs stochastic → more uncertainty needs more memory/verification.

### 4. Tools as primitive (easy)
Tools ground the model in the world (files, code, search), turning a predictor into a doer.

### 5. Compounding errors (detailed)
Each step conditions on prior (maybe wrong) steps → mitigate with verification, critics, checkpoints, short loops.

---

## Architectures

### 6. Single vs multi-agent (detailed)
Multi-agent helps for clear specialties/debate but adds coordination + failure surface. Default single well-tooled agent.

### 7. ReAct (detailed)
Interleave Reasoning (thought) and Acting (tool call) → grounded, debuggable trajectories.

### 8. Plan-and-execute (easy)
Make a full plan first, then execute steps → fewer LLM calls, good for known workflows.

### 9. Reflection / self-critique (code)
```python
answer = llm(task)
critique = llm(f"Find flaws in this answer:\n{answer}")
answer = llm(f"Improve given critique:\n{answer}\n{critique}")
```

---

## Tool Use

### 10. Tool schema / calling (code)
```python
tool = {
  "name": "search", "description": "web search",
  "parameters": {"type": "object", "properties": {"q": {"type": "string"}}}
}
call = llm_with_tools(prompt, [tool])   # -> {"name":"search","args":{"q":...}}
result = registry[call["name"]](**call["args"])
```

### 11. Tool errors / retries (detailed)
Validate args, catch exceptions, feed error back to the model to self-correct; cap retries.
```python
try:
    obs = tools[name](**args)
except Exception as e:
    obs = f"Error: {e}. Fix the arguments and retry."
```

### 12. Parallel tool calls (easy)
Independent calls run concurrently → lower latency; merge results before next reasoning step.

---

## Memory

### 13. Short vs long-term memory (detailed)
Short: conversation/scratchpad in context. Long: vector store / DB retrieved on demand.

### 14. Memory retrieval (code)
```python
def recall(query, store, k=5):
    q = embed(query)
    return store.search(q, k)   # top-k relevant past memories
```

### 15. Context management in agents (easy)
Summarize old steps, keep goal + recent observations, evict noise to stay in budget.

---

## Coding

### 16. ReAct loop (see Q1).
### 17. Reflection (see Q9).
### 18. Tool registry with error handling (see Q11).

---

## Judgment / Failure

### 19. Agent loops forever: no termination/progress check → step limit, loop detection, progress verification.
### 20. Agent picks wrong tool: unclear tool descriptions/too many tools → sharpen schemas, reduce toolset, few-shot examples.
### 21. Multi-agent overhead: coordination cost > benefit → collapse to single agent unless specialties clearly separate.
### 22. Agent hallucinates tool results: not actually calling tools → enforce structured calls, validate outputs, ground every claim.
