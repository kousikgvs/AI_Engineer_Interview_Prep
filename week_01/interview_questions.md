# Week 1 — Interview Questions: Probability, Statistics & Information Theory

> Click a question (▸) to expand the answer.

---

## Probability

<details>
<summary>1. What is a random variable? Discrete vs continuous.</summary>

A function mapping outcomes of a random process to numbers. Discrete RVs take countable values (dice, counts) described by a PMF; continuous RVs take values on a range (height, time) described by a PDF where probabilities come from integrals.
</details>

<details>
<summary>2. Define Bernoulli, Binomial, Gaussian, Poisson, Exponential + a use case.</summary>

Bernoulli = single yes/no trial (click or not). Binomial = number of successes in n trials (conversions). Gaussian = bell curve for aggregated noise (measurement error). Poisson = count of events per interval (requests/sec). Exponential = time between such events (latency gaps).
</details>

<details>
<summary>3. Derive the mean/variance of a Bernoulli / Binomial.</summary>

Bernoulli(p): E[X]=p, Var=p(1−p). Binomial(n,p) is a sum of n independent Bernoullis, so E[X]=np and Var=np(1−p) by linearity and independence.
</details>

<details>
<summary>4. State Bayes' Rule; compute P(disease | positive).</summary>

P(A|B) = P(B|A)P(A) / P(B). With base rate P(D), sensitivity P(+|D), and false-positive P(+|¬D): P(D|+) = sens·P(D) / [sens·P(D) + FP·(1−P(D))]. Low base rates make even accurate tests produce many false positives.
</details>

<details>
<summary>5. Law of Total Probability with an example.</summary>

P(B) = Σ P(B|Aᵢ)P(Aᵢ) over a partition {Aᵢ}. E.g., overall spam rate = P(spam|hasLink)P(hasLink) + P(spam|noLink)P(noLink).
</details>

<details>
<summary>6. What is a Markov chain? Stationary distribution?</summary>

A process where the next state depends only on the current state (memoryless). The stationary distribution π satisfies π = πP; it exists/ is unique when the chain is irreducible and aperiodic, and describes long-run time spent in each state.
</details>

<details>
<summary>7. Joint vs marginal vs conditional probability.</summary>

Joint P(A,B) = both happen; marginal P(A) = sum/integrate the joint over B; conditional P(A|B) = P(A,B)/P(B) = A given B is known.
</details>

<details>
<summary>8. E[X], Var[X]; prove Var = E[X²] − (E[X])².</summary>

Var = E[(X−μ)²] = E[X² − 2μX + μ²] = E[X²] − 2μE[X] + μ² = E[X²] − μ² since E[X]=μ.
</details>

## Statistics & Estimation

<details>
<summary>9. What is MLE? Derive MLE for a Gaussian mean.</summary>

MLE picks parameters maximizing the likelihood of observed data. For a Gaussian, maximize the log-likelihood; taking ∂/∂μ = 0 gives μ̂ = sample mean (1/n Σxᵢ).
</details>

<details>
<summary>10. MLE vs MAP — how does a prior change it?</summary>

MAP maximizes the posterior ∝ likelihood × prior. It adds prior belief that regularizes the estimate (e.g., a Gaussian prior → L2 regularization). With lots of data the prior washes out and MAP → MLE.
</details>

<details>
<summary>11. Explain the bias-variance tradeoff.</summary>

Total error = bias² + variance + irreducible noise. Simple models have high bias (underfit); complex models have high variance (overfit). You tune capacity/regularization to balance them.
</details>

<details>
<summary>12. What does a p-value mean — and not mean?</summary>

It's P(data at least this extreme | null hypothesis true). It is NOT the probability the null is true, nor the effect size or its importance. A small p-value just means the data are unlikely under the null.
</details>

<details>
<summary>13. What is a confidence interval? Interpret "95% CI".</summary>

A procedure that, across many samples, produces intervals containing the true parameter 95% of the time. It does NOT mean "95% probability the parameter is in this specific interval" (frequentist interpretation).
</details>

<details>
<summary>14. State the Central Limit Theorem and why it matters.</summary>

The mean of many i.i.d. samples is approximately Gaussian regardless of the underlying distribution (finite variance). It justifies using normal-based inference and why aggregated noise looks Gaussian.
</details>

<details>
<summary>15. Frequentist vs Bayesian interpretation.</summary>

Frequentist: probability = long-run frequency; parameters are fixed unknowns. Bayesian: probability = degree of belief; parameters have distributions updated via Bayes' rule with priors.
</details>

<details>
<summary>16. Type I vs Type II error.</summary>

Type I = false positive (reject a true null), rate α. Type II = false negative (fail to reject a false null), rate β; power = 1−β. There's a tradeoff between them.
</details>

## Information Theory

<details>
<summary>17. Define entropy H(X). When is it maximized?</summary>

H(X) = −Σ p(x) log p(x) — the average uncertainty/bits needed to encode X. Maximized by the uniform distribution (all outcomes equally likely).
</details>

<details>
<summary>18. What is cross-entropy vs log-loss?</summary>

Cross-entropy H(p,q) = −Σ p(x) log q(x) measures the cost of using predicted distribution q for true distribution p. Classification log-loss is exactly cross-entropy between the one-hot label and the predicted probabilities.
</details>

<details>
<summary>19. Derive cross-entropy loss from MLE.</summary>

Maximizing the likelihood of labels under the model = maximizing Σ log q(yᵢ) = minimizing −Σ log q(yᵢ), which is the cross-entropy loss. So minimizing cross-entropy IS maximum likelihood.
</details>

<details>
<summary>20. What is KL divergence? Symmetric? Where used?</summary>

KL(p‖q) = Σ p log(p/q) — extra bits from using q instead of p; ≥0, and NOT symmetric. Appears in VAEs (latent regularizer), RLHF/DPO (KL penalty to a reference policy), and distillation.
</details>

<details>
<summary>21. What is mutual information I(X;Y)?</summary>

The reduction in uncertainty about X from knowing Y: I(X;Y) = H(X) − H(X|Y) = KL between the joint and the product of marginals. Zero iff X and Y are independent.
</details>

<details>
<summary>22. Why is cross-entropy the loss for classification?</summary>

It's the MLE objective for categorical outputs, penalizes confident wrong predictions heavily, has clean gradients with softmax, and directly optimizes predicted probability of the true class.
</details>

## Applied / Decision-Making

<details>
<summary>23. 95% accuracy on a 99%-negative dataset — good?</summary>

No — a model predicting "negative" always scores 99%. With class imbalance, accuracy is misleading; use precision, recall, F1, PR-AUC, and look at the minority class.
</details>

<details>
<summary>24. Fermi: GPU-hours to train GPT-2?</summary>

Estimate via compute ≈ 6 × params × tokens FLOPs, divide by GPU throughput × utilization. Show the reasoning chain (params ~1.5B, tokens ~10s of billions, ~V100/A100 TFLOPs) — the interviewer grades the decomposition, not the exact number.
</details>

<details>
<summary>25. Frame "fine-tune vs RAG" as decision theory.</summary>

Compare expected value: cost (data, training, maintenance) vs benefit (accuracy, latency, freshness). RAG wins for changing/knowledge-heavy tasks; fine-tuning wins for fixed style/format/behavior. Decide under uncertainty by which reduces the dominant risk.
</details>

<details>
<summary>26. What is calibration? How to measure/fix it?</summary>

A model is calibrated if predicted probabilities match observed frequencies (of things it says are 80% likely, 80% happen). Measure with a reliability diagram/ECE; fix with temperature scaling or Platt/isotonic calibration.
</details>

## Coding

<details>
<summary>27. Implement a Naive Bayes spam classifier in NumPy.</summary>

Estimate class priors and per-word likelihoods (with Laplace smoothing) from counts; classify by summing log-priors + log-likelihoods per class and taking the argmax. Work in log space to avoid underflow.
</details>

<details>
<summary>28. Estimate parameters via MLE by hand and in code.</summary>

Write the log-likelihood, set its derivative to zero for closed forms (Gaussian mean/variance = sample mean/variance), or maximize numerically with gradient ascent when no closed form exists.
</details>

<details>
<summary>29. Compute/plot KL divergence between two Gaussians.</summary>

Use the closed form KL(N₀‖N₁) = log(σ₁/σ₀) + (σ₀²+(μ₀−μ₁)²)/(2σ₁²) − ½, and sweep a parameter to plot how divergence grows as the distributions separate.
</details>

## Failure / Judgment

<details>
<summary>30. Microsoft Tay — what failure modes?</summary>

An online-learning bot with no input filtering or reward guardrails was adversarially fed toxic data and adapted to it — a feedback-loop/data-poisoning failure. Fixes: input moderation, rate limits, no unfiltered online learning, human oversight.
</details>

<details>
<summary>31. "Frequentist or Bayesian for this classification problem?"</summary>

Bayesian when you have informative priors, small data, or need calibrated uncertainty; frequentist when data is plentiful and you want simple point estimates/tests. Tradeoff: Bayesian gives uncertainty but costs modeling/compute; rejected the other when its assumptions don't fit the data regime.
</details>
