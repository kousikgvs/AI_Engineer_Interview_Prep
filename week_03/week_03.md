# Week 3 — Classical ML & Engineering Judgment

**Block:** A — Math + ML Foundations  
**Goal:** Build ML intuition from the ground up. No sklearn allowed.

---

## What You Will Learn

### Supervised Learning from Scratch
- Linear Regression — normal equation and gradient descent solution
- Logistic Regression — sigmoid function, binary cross-entropy loss
- Regularization — L1 (Lasso), L2 (Ridge), ElasticNet — why they work geometrically
- Decision Trees — information gain and Gini impurity
- Ensemble Methods — Random Forests, Gradient Boosting, XGBoost intuition
- Support Vector Machines — maximum margin and the kernel trick
- k-Nearest Neighbors (kNN)

### Unsupervised Learning
- k-Means clustering
- DBSCAN
- Gaussian Mixture Models
- Dimensionality reduction: PCA, t-SNE, UMAP

### Model Evaluation
- Train / validation / test splits
- Cross-validation
- Precision, Recall, F1 score, AUC-ROC
- Calibration curves and confusion matrices
- When accuracy is a misleading metric (class imbalance, etc.)

### Engineering Judgment — First Principles
Every week you answer in writing:
1. What would you do?
2. Why?
3. What tradeoff are you making?
4. What alternative did you reject?

**This week:** "A client wants a churn prediction model. Should you start with logistic regression or XGBoost? Defend your choice."

---

## Project
- Build Logistic Regression from scratch in NumPy — no sklearn
- Train on a real tabular dataset (e.g., UCI Adult Income)
- Build a full evaluation pipeline: precision, recall, F1, AUC-ROC
- Do hyperparameter search by hand

---

## Paper to Read
- "Attention Is All You Need" — Vaswani et al. *(preview only; full deep-dive in Week 6)*

---

## Failure Friday
- Analyze: A real ML model that went into production and failed silently — what evaluation gaps caused the failure?

---

## Interview Questions This Week Prepares You For

<details>
<summary>"How does regularization prevent overfitting geometrically?"</summary>

It constrains weights to a smaller region (L2 ball / L1 diamond), shrinking them toward zero. Lower weight magnitude means lower model complexity/variance and smoother decision boundaries.
</details>

<details>
<summary>"Walk me through how you'd set up evaluation for a churn model"</summary>

Use a time-based split (past to predict future), metrics tied to business cost (recall on churners, PR-AUC), calibrated probabilities, slice evaluation, and post-deploy drift monitoring.
</details>

<details>
<summary>"Why might a model with 95% accuracy still be useless?"</summary>

Class imbalance — predicting the majority class always scores high while missing the important minority. Use precision, recall, F1, and PR-AUC instead.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"XGBoost or neural network for this tabular dataset?"</strong></summary>

**What I'd do:** XGBoost — gradient-boosted trees usually beat neural nets on tabular data and train faster.
**Why:** trees handle mixed types, missing values, and feature interactions with little tuning.
**Tradeoff:** less flexible for unstructured/multimodal inputs than a neural net.
**Rejected:** a neural net unless the data is large, unstructured, or needs end-to-end feature learning.
</details>
