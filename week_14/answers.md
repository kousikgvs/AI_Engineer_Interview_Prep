# Week 14 — Answers with Code: Evaluation & Reliability Engineering

> Golden datasets, LLM-as-judge, metrics, and guardrails with code.

---

## Evaluation Systems

### 1. Why eval is hardest (detailed)
Open-ended, non-deterministic output with no single correct answer → must define quality, build golden data, handle subjectivity, catch regressions.

### 2. Good golden dataset (detailed)
Representative of real traffic, covers common + edge + hard slices, agreed rubrics, versioned, statistically meaningful size.

### 3. Slice testing (detailed + code)
Evaluate per subset — averages hide slice failures.
```python
for slice_name, subset in slices.items():
    acc = sum(evaluate(x) for x in subset) / len(subset)
    print(slice_name, acc)   # catch a bad slice hidden by good average
```

### 4. LLM-as-judge biases (detailed)
Position, verbosity, self-preference bias; inconsistency; fooled by confident-wrong. Mitigate: clear rubric, pairwise, randomize order, calibrate to humans.

### 5. Reference-based vs reference-free (easy)
Compare to gold (BLEU/ROUGE/exact) vs judge quality directly (LLM-judge, groundedness).

### 6. BLEU/ROUGE/BERTScore limits (easy)
N-gram overlap misses meaning; BERTScore better but still no factuality/reasoning.

---

## Metrics

### 7. Precision / recall / F1 (code)
```python
def prf(tp, fp, fn):
    p = tp / (tp + fp); r = tp / (tp + fn)
    return p, r, 2 * p * r / (p + r)
```

### 8. LLM-as-judge implementation (code)
```python
RUBRIC = "Score 1-5 for factual accuracy. Respond with only the number."
def judge(question, answer):
    resp = strong_llm(f"{RUBRIC}\nQ: {question}\nA: {answer}")
    return int(resp.strip())
```

### 9. Pairwise comparison (detailed)
Ask judge which of A/B is better (randomize order, average both orders) → more reliable than absolute scores.

### 10. Faithfulness / groundedness (detailed)
Check every claim is supported by retrieved context (NLI or LLM-judge) → detects hallucination in RAG.

---

## Reliability

### 11. Guardrails (detailed + code)
Validate inputs/outputs; block PII, injection, unsafe content.
```python
def guard(output):
    if contains_pii(output): return "[redacted]"
    if not schema_valid(output): raise ValueError("bad format")
    return output
```

### 12. Regression testing (easy)
Run the golden set on every change in CI; fail the build if key metrics drop.

### 13. A/B testing / online eval (easy)
Compare variants on live traffic with real metrics (thumbs, task success), guardrail metrics for safety.

### 14. Handling non-determinism (detailed)
Fix seed/temperature=0 for tests, or run N samples and assert on distribution/pass-rate, not exact string.

### 15. Human-in-the-loop (easy)
Sample outputs for human review; use disagreements to refine rubric and calibrate the judge.

---

## Coding

### 16. LLM-as-judge (see Q8).
### 17. Slice evaluation harness (see Q3).
### 18. Output guardrail (see Q11).

---

## Judgment / Failure

### 19. Metrics look great, users unhappy: eval set unrepresentative → sample real traffic, add hard slices, online metrics.
### 20. LLM-judge inconsistent: vague rubric/position bias → concrete rubric, pairwise + order swap, calibrate to humans.
### 21. Regression slipped to prod: no eval gate → golden set in CI, block on metric drops, canary rollout.
