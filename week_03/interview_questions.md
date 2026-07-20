# Week 3 — Interview Questions: Classical ML & Engineering Judgment

> Sourced from DS/MLE interview banks (Glassdoor, StrataScratch, company screens). Classic ML still shows up constantly.

---

## Supervised Learning
1. Derive linear regression via the normal equation. When would you use gradient descent instead?
2. Derive logistic regression: sigmoid + binary cross-entropy. Why not use MSE for classification?
3. Explain L1 vs L2 regularization geometrically. Why does L1 produce sparsity?
4. What is ElasticNet and when do you use it?
5. How does a decision tree split? Explain information gain and Gini impurity.
6. Random Forest vs Gradient Boosting vs XGBoost — how do they differ?
7. Explain the bias-variance behavior of bagging vs boosting.
8. What is the kernel trick in SVMs? What problem does the maximum margin solve?
9. How does kNN work and what are its weaknesses (curse of dimensionality)?

## Unsupervised Learning
10. How does k-Means work? How do you choose k? What are its assumptions/failure modes?
11. k-Means vs DBSCAN vs Gaussian Mixture Models — when to use each?
12. PCA vs t-SNE vs UMAP — what is each good/bad for?

## Model Evaluation
13. Define precision, recall, F1, and AUC-ROC. When do you optimize for each?
14. **Why might a model with 95% accuracy be useless?** (class imbalance)
15. What is cross-validation and why do it? k-fold vs stratified vs time-series split.
16. What is a calibration curve and why does calibration matter?
17. What is data leakage and how do you prevent it?
18. Precision-recall curve vs ROC — when is PR more informative?

## Applied / Judgment
19. A client wants a churn model — logistic regression or XGBoost? Defend it.
20. Walk me through how you'd set up evaluation for a churn model end-to-end.
21. How does regularization prevent overfitting geometrically?

## Coding
22. Implement logistic regression from scratch in NumPy (no sklearn).
23. Build a full eval pipeline: precision, recall, F1, AUC-ROC.
24. Do a by-hand hyperparameter search and explain your grid.

## Failure Analysis
25. A model went to production and failed silently — what evaluation gaps cause this?
