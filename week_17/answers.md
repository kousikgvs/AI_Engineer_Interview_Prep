# Week 17 — Answers with Code: Reasoning Models, Agentic RL & Research Thinking

> Reasoning models, test-time compute, verifiers with code.

---

## Reasoning Models

### 1. CoT as emergent (easy)
Reasoning benefit appears with scale/training, not an explicit module.

### 2. Trained vs prompted CoT (detailed)
Prompted: elicited by "think step by step". Trained: RL/SFT on reasoning traces → reasons by default, more reliably.

### 3. PRMs vs ORMs (detailed)
ORM scores final answer only; PRM scores each step → denser signal, catches wrong steps early, costlier to label.

### 4. o1-style reasoning (detailed)
Spend more inference compute searching/evaluating multiple reasoning paths before answering — latency for accuracy.

### 5. Test-time compute scaling (detailed)
More inference compute (longer reasoning, more samples, search) boosts hard-task accuracy — a scaling axis beyond model size.

### 6. Majority vote vs best-of-N (detailed + code)
```python
# majority vote
from collections import Counter
def majority(samples): return Counter(samples).most_common(1)[0][0]
# best-of-N with a verifier
def best_of_n(samples, verifier): return max(samples, key=verifier)
```

---

## Agentic RL

### 7. RL for reasoning (RLVR) (detailed)
Reward = verifiable correctness (math/code tests). No reward model needed — the environment checks answers.
```python
def reward(answer, test_cases):
    return 1.0 if all(run(answer, t) for t in test_cases) else 0.0
```

### 8. Why verifiable rewards work (detailed)
Ground-truth checkable rewards avoid reward-model gaming → clean signal for reasoning/coding.

### 9. GRPO (detailed)
Group Relative Policy Optimization: sample a group of answers, use their mean reward as the baseline (no separate value network) → simpler, memory-efficient RL (used by DeepSeek).

### 10. Exploration vs exploitation (easy)
Sample diverse reasoning (temperature) to discover better solutions vs commit to known-good paths.

---

## Verification & Search

### 11. Self-verification (code)
```python
answer = llm(problem)
check = llm(f"Verify step-by-step; is this correct? {problem}\n{answer}")
if "incorrect" in check.lower(): answer = llm(f"Retry: {problem}\nHint: {check}")
```

### 12. Process reward for search (detailed)
Use a PRM to score partial reasoning at each node → guide beam/tree search toward promising paths.

### 13. Monte Carlo Tree Search for reasoning (detailed)
Expand/evaluate/backpropagate over a tree of reasoning steps (AlphaGo-style) → strong on hard search problems, expensive.

---

## Research Thinking

### 14. Reading papers efficiently (easy)
Abstract → figures → results → method; ask what problem, what's new, what's the evidence.

### 15. Ablation studies (detailed)
Remove one component at a time to attribute the gain → shows what actually matters.

### 16. Reproducibility (easy)
Fix seeds, log configs/data versions, report variance across runs.

---

## Coding

### 17. Best-of-N + verifier (see Q6).
### 18. Verifiable reward function (see Q7).
### 19. Self-verification loop (see Q11).

---

## Judgment / Failure

### 20. Reasoning model slow/expensive: over-reasoning simple queries → route easy queries to fast path, cap reasoning budget.
### 21. Reward hacking in RLVR: tests too weak → strengthen/expand test cases, hidden tests.
### 22. Best-of-N no gain: verifier weak/miscalibrated → improve verifier, use majority vote as fallback.
