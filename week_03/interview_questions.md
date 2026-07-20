# Week 3 — Interview Questions: Classical ML & Engineering Judgment

> Click a question (▸) to expand the answer.

---

## Supervised Learning

<details>
<summary>1. Derive linear regression via the normal equation; when use GD?</summary>

Minimizing squared error gives θ = (XᵀX)⁻¹Xᵀy in closed form. Use gradient descent instead when X is huge (inverting XᵀX is O(n³)) or XᵀX is ill-conditioned/singular.
</details>

<details>
<summary>2. Derive logistic regression; why not MSE?</summary>

Model P(y=1)=σ(wᵀx); fit by minimizing binary cross-entropy (MLE for Bernoulli). MSE with a sigmoid is non-convex and has vanishing gradients when saturated, so cross-entropy is preferred.
</details>

<details>
<summary>3. L1 vs L2 regularization geometrically; why L1 sparsity?</summary>

L2 (ridge) is a circular constraint that shrinks weights smoothly; L1 (lasso) is a diamond whose corners lie on the axes, so the optimum often hits an axis → exactly-zero weights (feature selection).
</details>

<details>
<summary>4. What is ElasticNet?</summary>

A mix of L1 and L2 penalties. It gets L1's sparsity plus L2's stability with correlated features (where pure lasso arbitrarily picks one).
</details>

<details>
<summary>5. How does a decision tree split (info gain, Gini)?</summary>

It greedily picks the feature/threshold that most reduces impurity — entropy reduction (information gain) or Gini impurity — recursively partitioning the data.
</details>

<details>
<summary>6. Random Forest vs Gradient Boosting vs XGBoost?</summary>

RF: many independent deep trees on bootstrapped data, averaged (reduces variance). Boosting: sequential shallow trees each fixing prior errors (reduces bias). XGBoost: optimized boosting with regularization, second-order gradients, and speed tricks.
</details>

<details>
<summary>7. Bias-variance of bagging vs boosting?</summary>

Bagging mainly reduces variance (averaging decorrelated models); boosting mainly reduces bias (sequentially correcting errors) but can overfit if unregularized.
</details>

<details>
<summary>8. What is the kernel trick; what does max margin solve?</summary>

The kernel trick computes inner products in a high-dimensional space without explicitly mapping there, enabling nonlinear boundaries. Max margin picks the separating hyperplane with the largest gap to the nearest points, improving generalization.
</details>

<details>
<summary>9. How does kNN work; weaknesses?</summary>

Classify by majority vote of the k nearest training points. Weaknesses: slow at inference, memory-heavy, needs good distance/scaling, and degrades in high dimensions (curse of dimensionality).
</details>

## Unsupervised Learning

<details>
<summary>10. How does k-Means work; choose k; failure modes?</summary>

Alternately assign points to nearest centroid and recompute centroids. Choose k via elbow/silhouette. Fails on non-spherical/unequal clusters and is sensitive to init (use k-means++).
</details>

<details>
<summary>11. k-Means vs DBSCAN vs GMM?</summary>

k-Means: spherical, must set k. DBSCAN: density-based, finds arbitrary shapes and outliers, no k. GMM: soft probabilistic clusters (elliptical) via EM.
</details>

<details>
<summary>12. PCA vs t-SNE vs UMAP?</summary>

PCA: linear, fast, preserves global variance. t-SNE: nonlinear, great local structure for visualization, slow, no meaningful distances. UMAP: nonlinear, faster than t-SNE, better global structure.
</details>

## Model Evaluation

<details>
<summary>13. Define precision, recall, F1, AUC-ROC; when optimize each?</summary>

Precision = TP/(TP+FP) (cost of false alarms). Recall = TP/(TP+FN) (cost of misses). F1 = harmonic mean. AUC-ROC = ranking quality across thresholds. Optimize recall for cancer screening, precision for spam, F1 for balance.
</details>

<details>
<summary>14. Why can 95% accuracy be useless?</summary>

Class imbalance — predicting the majority always scores high while missing the important minority. Use precision/recall/F1/PR-AUC instead.
</details>

<details>
<summary>15. Cross-validation; k-fold vs stratified vs time-series?</summary>

CV estimates generalization by rotating train/val splits. Stratified preserves class ratios; time-series split respects temporal order (no future leakage).
</details>

<details>
<summary>16. What is a calibration curve; why care?</summary>

It plots predicted probability vs observed frequency. Calibration matters when you act on probabilities (thresholds, expected value, risk), not just rankings.
</details>

<details>
<summary>17. What is data leakage; prevent it?</summary>

When training data includes information unavailable at prediction time (target-derived features, future data, fitting scalers on the full set). Prevent by splitting first and fitting all preprocessing inside CV folds.
</details>

<details>
<summary>18. PR curve vs ROC — when is PR more informative?</summary>

On heavy class imbalance, ROC can look optimistic because true negatives dominate; the precision-recall curve focuses on the positive class and is more informative.
</details>

## Applied / Judgment

<details>
<summary>19. Churn model: logistic regression or XGBoost?</summary>

Start with logistic regression for a fast, interpretable baseline; move to XGBoost if nonlinear interactions and accuracy justify the complexity. Tradeoff: interpretability/latency vs accuracy.
</details>

<details>
<summary>20. Set up evaluation for a churn model end-to-end.</summary>

Time-based split (train on past, test on future), pick metrics tied to business cost (recall on churners, PR-AUC), calibrate probabilities, evaluate on slices, and monitor drift post-deploy.
</details>

<details>
<summary>21. How does regularization prevent overfitting geometrically?</summary>

It constrains weights to a smaller region (ball/diamond), shrinking them toward zero, which reduces model complexity/variance and yields smoother decision boundaries.
</details>

## Coding

<details>
<summary>22. Implement logistic regression in NumPy.</summary>

Compute z=Xw, probabilities σ(z), cross-entropy loss, gradient Xᵀ(σ(z)−y)/n, and update w via gradient descent; add L2 by adding λw to the gradient.
</details>

<details>
<summary>23. Build a full eval pipeline (precision/recall/F1/AUC).</summary>

From predicted probabilities and labels, threshold for the confusion matrix (precision/recall/F1) and sweep thresholds for ROC/PR curves and their AUCs.
</details>

<details>
<summary>24. By-hand hyperparameter search.</summary>

Define a grid (learning rate, regularization, depth), evaluate each with cross-validation on a fixed metric, pick the best, and confirm on a held-out test set.
</details>

## Failure Analysis

<details>
<summary>25. A model failed silently in production — why?</summary>

Usually evaluation gaps: no monitoring, wrong metric (accuracy under imbalance), train/serve skew, data drift, or leakage inflating offline scores. Fix with slice evals, drift detection, and shadow testing.
</details>
