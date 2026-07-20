# Week 1 — Answers with Code: Probability, Statistics & Information Theory

> Detailed answers with code. Math-heavy items get derivations; formulas shown in code.

---

## Probability

### 1. Random variable (easy)
A function mapping outcomes to numbers. Discrete → PMF; continuous → PDF.

### 2. Distributions + use cases
```python
import numpy as np
from scipy import stats
stats.bernoulli(p=0.3).rvs(5)     # click / no-click
stats.binom(n=10, p=0.3).rvs(5)   # #conversions in 10 trials
stats.norm(0, 1).rvs(5)           # aggregated noise
stats.poisson(mu=4).rvs(5)        # events per interval (req/sec)
stats.expon(scale=1/4).rvs(5)     # time between events (latency gaps)
```

### 3. Mean/variance of Bernoulli/Binomial (derivation)
Bernoulli(p): `E[X]=p`, `Var=p(1-p)`. Binomial = sum of n Bernoullis → `E=np`, `Var=np(1-p)`.

### 4. Bayes' Rule — disease test (tricky, base-rate trap)
```python
def posterior(base, sens, fp):
    # P(D|+) = sens*base / (sens*base + fp*(1-base))
    return sens*base / (sens*base + fp*(1-base))

print(posterior(base=0.01, sens=0.99, fp=0.05))  # ~0.167 — low despite 99% test!
```
Even an accurate test yields many false positives when the disease is rare.

### 5. Law of Total Probability
`P(B) = Σ P(B|Aᵢ) P(Aᵢ)` over a partition. E.g., overall spam = P(spam|link)P(link)+P(spam|no link)P(no link).

### 6. Markov chain + stationary distribution (detailed)
```python
import numpy as np
P = np.array([[0.9, 0.1],
              [0.5, 0.5]])          # transition matrix
# stationary π solves π = πP  → left eigenvector with eigenvalue 1
vals, vecs = np.linalg.eig(P.T)
pi = np.real(vecs[:, np.isclose(vals, 1)][:, 0])
pi = pi / pi.sum()
print(pi)   # long-run fraction of time in each state
```

### 7. Joint/marginal/conditional (easy)
`P(A,B)` both; `P(A)=Σ_B P(A,B)`; `P(A|B)=P(A,B)/P(B)`.

### 8. Var = E[X²] − (E[X])² (derivation)
`Var = E[(X-μ)²] = E[X²] - 2μE[X] + μ² = E[X²] - μ²`.

---

## Statistics & Estimation

### 9. MLE for Gaussian mean (derivation + code)
Maximize log-likelihood; `∂/∂μ = 0 → μ̂ = mean`.
```python
x = np.random.normal(5, 2, size=1000)
mu_hat = x.mean()          # MLE mean
var_hat = x.var()          # MLE variance (biased, /n)
```

### 10. MLE vs MAP (detailed)
MAP maximizes `likelihood × prior`. A Gaussian prior on weights = L2 regularization. With lots of data MAP → MLE.

### 11. Bias-variance (easy)
`Error = bias² + variance + noise`. Simple → underfit; complex → overfit.

### 12. p-value (tricky — commonly misunderstood)
It's `P(data this extreme | null true)`. NOT the probability the null is true, nor effect size.

### 13. Confidence interval (tricky)
"95% CI" = the procedure captures the true parameter 95% of the time across samples — not "95% chance it's in this one interval."

### 14. Central Limit Theorem
```python
means = [np.random.exponential(1, 100).mean() for _ in range(5000)]
# distribution of means is ~Gaussian even though data is exponential
```

### 15. Frequentist vs Bayesian (easy)
Frequentist: probability = long-run frequency, params fixed. Bayesian: probability = belief, params have distributions.

### 16. Type I vs II (easy)
Type I = false positive (α); Type II = false negative (β); power = 1−β.

---

## Information Theory

### 17. Entropy (code)
```python
def entropy(p):
    p = np.asarray(p); p = p[p > 0]
    return -np.sum(p * np.log2(p))    # bits
entropy([0.5, 0.5])   # 1.0 (max for 2 outcomes)
```

### 18. Cross-entropy vs log-loss (easy)
Log-loss IS cross-entropy between the one-hot label and predicted probabilities.

### 19. Derive cross-entropy from MLE (detailed — very common)
Maximizing label likelihood `Σ log q(yᵢ)` = minimizing `-Σ log q(yᵢ)` = cross-entropy. So CE minimization *is* MLE.
```python
def cross_entropy(y_true_onehot, y_prob, eps=1e-12):
    return -np.sum(y_true_onehot * np.log(y_prob + eps))
```

### 20. KL divergence (detailed — appears everywhere)
```python
def kl(p, q, eps=1e-12):
    p, q = np.asarray(p), np.asarray(q)
    return np.sum(p * np.log((p + eps) / (q + eps)))   # asymmetric, >= 0
```
Appears in VAEs (latent regularizer), RLHF/DPO (KL penalty to reference policy), distillation.

### 21. Mutual information
`I(X;Y) = H(X) - H(X|Y)`; zero iff independent.

### 22. Why CE for classification (easy)
It's the MLE objective for categoricals, penalizes confident wrong answers, and has clean softmax gradients.

---

## Applied / Decision-Making

### 23. 95% accuracy trap (tricky)
On 99%-negative data, always-negative scores 99%. Use precision/recall/F1/PR-AUC.

### 24. Fermi: GPU-hours for GPT-2
`compute ≈ 6 · params · tokens`. Divide by GPU FLOPs × utilization. Show the decomposition.

### 25. Fine-tune vs RAG as decision theory
Compare expected value: RAG for changing knowledge; fine-tune for fixed style. Pick what reduces the dominant risk.

### 26. Calibration (detailed)
```python
from sklearn.calibration import calibration_curve
frac_pos, mean_pred = calibration_curve(y, probs, n_bins=10)
# perfect calibration lies on the diagonal; fix with temperature scaling
```

---

## Coding

### 27. Naive Bayes spam classifier (NumPy)
```python
import numpy as np
class NB:
    def fit(self, X, y):                       # X: bag-of-words counts
        self.classes = np.unique(y)
        self.logprior, self.loglik = {}, {}
        for c in self.classes:
            Xc = X[y == c]
            self.logprior[c] = np.log(len(Xc) / len(X))
            counts = Xc.sum(0) + 1              # Laplace smoothing
            self.loglik[c] = np.log(counts / counts.sum())
    def predict(self, X):
        out = []
        for x in X:
            scores = {c: self.logprior[c] + (x * self.loglik[c]).sum()
                      for c in self.classes}
            out.append(max(scores, key=scores.get))
        return np.array(out)
```

### 28. MLE estimation (see Q9).

### 29. KL between two Gaussians (closed form)
```python
def kl_gauss(m0, s0, m1, s1):
    return np.log(s1/s0) + (s0**2 + (m0-m1)**2)/(2*s1**2) - 0.5
```

---

## Failure / Judgment

### 30. Microsoft Tay (detailed)
Online learning + no input filtering + no reward guardrails → adversarial data poisoning. Fix: moderation, rate limits, no unfiltered online learning, human oversight.

### 31. Frequentist vs Bayesian judgment
Bayesian for small data / informative priors / calibrated uncertainty; frequentist for plentiful data and simple estimates. Tradeoff: Bayesian gives uncertainty at modeling/compute cost.
