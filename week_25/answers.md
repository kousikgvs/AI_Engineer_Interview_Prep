# Week 25 — Answers with Code: AI Ethics, Responsible AI & Generative Foundations

> Generative foundations (with code) + responsible AI (conceptual).

---

## Generative Foundations

### 1. Generative vs discriminative (detailed)
Generative learns P(x) (or P(x|context)) → samples new data. Discriminative learns P(y|x) → labels existing data.

### 2. Latent variables / sampling (detailed)
Hidden factors explaining data; learn a distribution over them, sample latents, decode → varied outputs.

### 3. Generative model family (easy)
Autoencoders → VAEs (probabilistic latent) → GANs (adversarial) → Diffusion (denoising) → Autoregressive transformers (next-token).

### 4. Next-token as generative (detailed)
Modeling P(next | previous) factorizes the joint sequence distribution; sampling generates coherent text.
```python
# joint via chain rule: P(x1..xn) = Π P(xi | x1..x_{i-1})
logp = sum(log_softmax(model(x[:i]))[x[i]] for i in range(1, len(x)))
```

---

## Bias & Fairness

### 5. Where bias enters (detailed)
Data (unrepresentative/historical), labeling (annotator bias), objective (what you optimize), deployment context. Every stage.

### 6. Fairness metrics (detailed)
Demographic parity (equal positive rate), equalized odds (equal TPR/FPR across groups), individual fairness. They can conflict — pick per context.
```python
def demographic_parity(pred, group):
    return abs(pred[group==1].mean() - pred[group==0].mean())  # want ~0
```

### 7. Bias mitigation (easy)
Pre (rebalance/reweight data), in (fairness constraints in training), post (adjust thresholds per group).

### 8. Fairness trade-offs (detailed)
Impossible to satisfy all fairness definitions simultaneously (except degenerate cases); fairness often trades against accuracy → context-driven choice.

---

## Responsible AI

### 9. Hallucination responsibility (easy)
Ground with RAG, cite sources, express uncertainty, human review for high-stakes, clear disclaimers.

### 10. Privacy / PII (detailed)
Don't train on/expose PII; redact inputs, filter outputs, data minimization, differential privacy for training.

### 11. Transparency / explainability (easy)
Document model cards, data sources, limitations; provide reasoning/citations; disclose AI use.

### 12. Membership inference / memorization (detailed)
Models can leak training data. Mitigate: dedup training data, differential privacy, output filtering, canary testing.

### 13. Model / data cards (easy)
Standardized docs of intended use, training data, performance across slices, and limitations → accountability.

---

## Governance & Safety

### 14. AI regulation (EU AI Act) (easy)
Risk-tiered rules (unacceptable/high/limited/minimal); high-risk needs documentation, oversight, transparency.

### 15. Red-teaming (detailed)
Adversarially probe for harmful/unsafe/biased outputs before launch → find failures proactively.
```python
for attack in jailbreaks + bias_probes + injection_tests:
    out = model(attack)
    if unsafe(out): log_finding(attack, out)   # fix before shipping
```

### 16. Human oversight (easy)
Human-in-the-loop for high-stakes decisions; clear escalation and override paths.

### 17. Dual-use / misuse (detailed)
Powerful models enable misuse (disinfo, malware); mitigate with usage policies, safety training, monitoring, access controls.

---

## Coding

### 18. Fairness metric (see Q6).
### 19. Red-team harness (see Q15).
### 20. Autoregressive log-likelihood (see Q4).

---

## Judgment / Failure

### 21. Model biased against a group: skewed data/objective → audit slices, rebalance, fairness constraints, monitor post-deploy.
### 22. Model leaked training data: memorization → dedup, differential privacy, output filtering.
### 23. Shipped without safety review: no red-teaming → adversarial testing + eval gates before launch.
