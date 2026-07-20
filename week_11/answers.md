# Week 11 — Answers with Code: LLM Engineering & Context Engineering

> Prompting techniques, structured output, and context management.

---

## Prompt Engineering

### 1. Zero/few/many-shot (easy)
Instructions only / a few examples / many examples. More examples help format-sensitive/hard tasks at higher token cost.

### 2. Why CoT works (detailed)
"Think step by step" lets the model spend compute on intermediate steps and condition each on the last → better multi-step/arithmetic reasoning.

### 3. Self-consistency (detailed + code)
Sample N reasoning paths, majority-vote the answer.
```python
from collections import Counter
answers = [extract(llm(prompt, temperature=0.7)) for _ in range(N)]
final = Counter(answers).most_common(1)[0][0]
```

### 4. ToT vs CoT (easy)
CoT = one linear chain; ToT = branching tree with evaluation/backtracking for search/planning.

### 5. Prompt chaining (easy)
Split a task into sequential prompts; each output feeds the next — reliable, debuggable.

### 6. Prompt compression (easy)
Same output with fewer tokens (concise instructions, trimmed context) → lower cost/latency.

### 7. Robust system prompt (detailed)
Role + explicit instructions + output format + constraints/guardrails + few-shot + negatives, tested on edge cases and versioned.

---

## Structured Output

### 8. Force JSON output (detailed + code)
Use schema-constrained decoding or validate+retry.
```python
from pydantic import BaseModel, ValidationError
class Out(BaseModel): name: str; age: int
def structured(prompt):
    for _ in range(3):
        raw = llm(prompt + "\nReturn JSON only.")
        try: return Out.model_validate_json(raw)
        except ValidationError: continue
    raise ValueError("no valid JSON")
```

### 9. Constrained decoding / grammars (detailed)
Restrict logits to tokens allowed by a grammar (JSON schema/regex) so output is always valid by construction.

### 10. Function/tool calling (detailed + code)
Model emits a structured call; you execute and feed the result back.
```python
tools = [{"name": "get_weather", "parameters": {"city": "str"}}]
call = llm_with_tools(prompt, tools)      # {"name":..., "args":...}
if call: result = registry[call["name"]](**call["args"])
```

---

## Context Engineering

### 11. Context window management (detailed)
Prioritize: system prompt + recent turns + retrieved chunks; summarize/evict old turns; budget tokens.

### 12. Lost in the middle (detailed)
Models attend best to start/end of context → put key info there, rerank retrieved chunks by relevance.

### 13. Conversation memory (code)
```python
def build_context(system, history, new, max_tokens):
    ctx = [system]; budget = max_tokens - count(system) - count(new)
    for turn in reversed(history):        # keep most recent that fit
        if count(turn) > budget: break
        ctx.insert(1, turn); budget -= count(turn)
    return ctx + [new]
```

### 14. Summarization memory (easy)
Periodically compress old turns into a running summary to stay within the window.

### 15. Caching / prompt caching (easy)
Reuse KV for a static prefix (system prompt/few-shot) across requests → lower latency/cost.

---

## Coding

### 16. Self-consistency (see Q3).
### 17. JSON validate+retry (see Q8).
### 18. Context builder with token budget (see Q13).

---

## Judgment / Failure

### 19. Model ignores instructions: buried in long context → move to start/end, simplify, use format constraints.
### 20. JSON parsing breaks: free-form generation → constrained decoding or validate+retry.
### 21. Costs explode: huge prompts/few-shot every call → prompt caching, compression, RAG instead of stuffing.
