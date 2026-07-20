# Week 3 — Answers with Code: Classical ML & Engineering Judgment

> Answers with NumPy/sklearn code. Derivations for tricky ones.

---

## Supervised Learning

### 1. Linear regression — normal equation (derivation + code)
Minimizing squared error → `θ = (XᵀX)⁻¹Xᵀy`.
```python
import numpy as np
def normal_eq(X, y):
    return np.linalg.pinv(X.T @ X) @ X.T @ y
```
Use gradient descent when X is huge (inversion is O(n³)) or XᵀX is ill-conditioned.

### 2. Logistic regression — why not MSE (detailed)
Sigmoid + binary cross-entropy is convex with clean gradients; MSE+sigmoid is non-convex and saturates.
```python
def sigmoid(z): return 1/(1+np.exp(-z))
def bce_grad(X, y, w):
    return X.T @ (sigmoid(X @ w) - y) / len(y)
```

### 3. L1 vs L2 geometry (detailed — sparsity)
L2 = circular constraint (smooth shrink); L1 = diamond whose axis corners drive weights to exactly zero (feature selection).

### 4. ElasticNet (easy)
Mix of L1+L2: sparsity plus stability with correlated features.

### 5. Tree splits (easy)
Greedily pick the feature/threshold maximizing information gain (entropy drop) or Gini reduction.

### 6. RF vs GBM vs XGBoost (detailed)
RF = independent deep trees averaged (↓variance). Boosting = sequential shallow trees fixing errors (↓bias). XGBoost = regularized, 2nd-order, optimized boosting.
```python
from xgboost import XGBClassifier
clf = XGBClassifier(n_estimators=300, max_depth=4, learning_rate=0.05)
```

### 7. Bagging vs boosting bias/variance (easy)
Bagging ↓variance; boosting ↓bias (can overfit unregularized).

### 8. Kernel trick / max margin (detailed)
Kernels compute inner products in high-dim space implicitly; max margin picks the widest-gap separator for generalization.

### 9. kNN weaknesses (easy)
Slow inference, memory-heavy, needs scaling, curse of dimensionality.

---

## Unsupervised

### 10. k-Means (code)
```python
from sklearn.cluster import KMeans
km = KMeans(n_clusters=3, n_init="auto").fit(X)   # choose k via elbow/silhouette
```
Fails on non-spherical/unequal clusters; sensitive to init (use k-means++).

### 11. k-Means vs DBSCAN vs GMM (easy)
Spherical+set-k vs density+arbitrary-shape vs soft probabilistic (EM).

### 12. PCA vs t-SNE vs UMAP (easy)
Linear/global vs nonlinear-local vs nonlinear faster-global.

---

## Model Evaluation

### 13. Precision/Recall/F1/AUC (detailed)
```python
from sklearn.metrics import precision_score, recall_score, f1_score, roc_auc_score
# precision = TP/(TP+FP); recall = TP/(TP+FN); F1 = harmonic mean; AUC = ranking
```
Optimize recall for screening, precision for spam, F1 for balance.

### 14. 95% accuracy useless (tricky)
Class imbalance — majority-class predictor scores high. Use PR metrics.

### 15. Cross-validation (code)
```python
from sklearn.model_selection import StratifiedKFold, cross_val_score
cross_val_score(clf, X, y, cv=StratifiedKFold(5))
```
Use time-series split for temporal data (no future leakage).

### 16. Calibration (detailed)
Predicted probs should match observed frequency. Fix with temperature/Platt/isotonic.

### 17. Data leakage (detailed)
Info unavailable at prediction time (target-derived features, future data, scaler fit on full set). Split first; fit preprocessing inside CV folds.

### 18. PR vs ROC (tricky)
On imbalance, ROC looks optimistic (TN dominate); PR focuses on the positive class.

---

## Applied / Judgment

### 19. Churn: LR or XGBoost
Start with LR (fast, interpretable baseline); move to XGBoost if nonlinearities/accuracy justify it.

### 20. Churn evaluation setup
Time-based split, cost-tied metrics (recall/PR-AUC), calibrated probs, slice eval, drift monitoring.

### 21. Regularization geometry (see Q3).

---

## Coding

### 22. Logistic regression from scratch
```python
def fit_logreg(X, y, lr=0.1, epochs=500, l2=0.0):
    w = np.zeros(X.shape[1])
    for _ in range(epochs):
        g = X.T @ (sigmoid(X @ w) - y) / len(y) + l2 * w
        w -= lr * g
    return w
```

### 23. Full eval pipeline
```python
from sklearn.metrics import classification_report, roc_auc_score
probs = sigmoid(X_test @ w)
print(classification_report(y_test, probs > 0.5))
print("AUC", roc_auc_score(y_test, probs))
```

### 24. Manual hyperparameter search
Grid over lr/l2/depth, evaluate with CV on a fixed metric, confirm on held-out test.

---

## Failure

### 25. Silent production failure (detailed)
Evaluation gaps: no monitoring, wrong metric under imbalance, train/serve skew, drift, leakage. Fix with slice evals, drift detection, shadow testing.
