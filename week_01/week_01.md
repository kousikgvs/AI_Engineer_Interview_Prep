# Week 1 — Probability, Statistics & Information Theory

**Block:** A — Math + ML Foundations  
**Goal:** Understand *why* models work, not just how to use them.

---

## What You Will Learn

### Probability
- What are random variables and probability spaces
- Bernoulli, Binomial, Gaussian (Normal), Uniform, Poisson, Exponential distributions
- Mean, variance, and expected value
- Joint, marginal, and conditional probability
- Bayes' Rule — how to update beliefs with new evidence
- Law of Total Probability
- Markov Chains — states, transitions, and steady-state behavior

### Statistics
- Maximum Likelihood Estimation (MLE) — find the best parameters for your data
- Maximum A Posteriori (MAP) — like MLE but with a prior belief
- Bias-Variance tradeoff — why simple models underfit and complex models overfit
- Confidence intervals and hypothesis testing
- What p-values actually mean (and what they don't)
- Frequentist vs Bayesian thinking
- Central Limit Theorem

### Information Theory
- Entropy H(X) — how to measure uncertainty
- Cross-entropy — how it connects to loss functions in training
- KL Divergence — how to measure the difference between two distributions
- Mutual Information — how much two variables share
- Why cross-entropy is used as the loss for classification (with derivation)

### Decision Making Under Uncertainty
- Expected value calculations
- Bayesian reasoning in real problems
- Fermi estimation (rough estimates with limited info)
- Calibration — knowing what you don't know

---

## Project
- Build a Bayesian spam classifier using only NumPy (no ML libraries)
- Implement MLE parameter estimation by hand
- Visualize Gaussian distributions and KL divergence

---

## Paper to Read
- "A Few Useful Things to Know About Machine Learning" — Pedro Domingos

---

## Failure Friday
- Analyze: **Microsoft Tay (2016)** — what probabilistic failure modes caused this chatbot to go wrong?

---

## Interview Questions This Week Prepares You For
- "Explain KL divergence and where it appears in training"
- "Derive the cross-entropy loss from MLE"
- "What is the bias-variance tradeoff?"

---

## Engineering Judgment Question
**"Should we use a Frequentist or Bayesian approach for this classification problem?"**  
Write your answer covering: what you would do, why, what tradeoff you are making, and what alternative you rejected.
