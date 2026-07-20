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
- "How does regularization prevent overfitting geometrically?"
- "Walk me through how you would set up evaluation for a churn model"
- "Why might a model with 95% accuracy still be useless?"

---

## Engineering Judgment Question
**"XGBoost or neural network for this tabular dataset?"**  
Write your answer covering: what you would do, why, what tradeoff you are making, and what alternative you rejected.
