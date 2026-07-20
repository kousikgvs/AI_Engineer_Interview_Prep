# Week 1 — Interview Questions: Probability, Statistics & Information Theory

> Sourced from ML/DS interview banks (Glassdoor, "Cracking the ML Interview", company screens). Expect derivations, not just definitions.

---

## Probability
1. What is a random variable? Discrete vs continuous.
2. Define the Bernoulli, Binomial, Gaussian, Poisson, and Exponential distributions and give a real use case for each.
3. Derive the mean and variance of a Bernoulli / Binomial.
4. State Bayes' Rule. Given a disease test with known sensitivity/specificity and base rate, compute P(disease | positive).
5. Explain the Law of Total Probability with an example.
6. What is a Markov chain? What is a stationary distribution and when does it exist?
7. Difference between joint, marginal, and conditional probability.
8. What is E[X] and Var[X]? Prove Var[X] = E[X²] − (E[X])².

## Statistics & Estimation
9. What is Maximum Likelihood Estimation? Derive the MLE for the mean of a Gaussian.
10. MLE vs MAP — how does a prior change the estimate?
11. Explain the bias-variance tradeoff. How does it relate to under/overfitting?
12. What does a p-value actually mean? What does it NOT mean?
13. What is a confidence interval? Interpret "95% CI" correctly.
14. State the Central Limit Theorem and why it matters.
15. Frequentist vs Bayesian interpretation of probability.
16. What is a Type I vs Type II error?

## Information Theory
17. Define entropy H(X). When is it maximized?
18. What is cross-entropy and how does it relate to log-loss?
19. **Derive the cross-entropy loss from MLE** (very common).
20. What is KL divergence? Is it symmetric? Where does it appear in ML (VAEs, RLHF, distillation)?
21. What is mutual information I(X;Y)?
22. Why is cross-entropy the loss function for classification?

## Applied / Decision-Making
23. A model has 95% accuracy on a 99%-negative dataset — is it good? (class imbalance)
24. Fermi estimate: how many GPU-hours to train GPT-2?
25. Frame "fine-tune vs RAG" as a decision-theory problem.
26. What does calibration mean for a classifier? How do you measure/fix it?

## Coding
27. Implement a Bayesian (Naive Bayes) spam classifier in NumPy only.
28. Estimate parameters via MLE by hand and in code.
29. Compute and plot KL divergence between two Gaussians.

## Failure / Judgment
30. Microsoft Tay (2016) — what probabilistic/feedback failure modes caused it?
31. "Frequentist or Bayesian for this classification problem?" — defend your choice.
