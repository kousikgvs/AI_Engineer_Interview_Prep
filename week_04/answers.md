# Week 4 — Answers with Code: Deep Learning Foundations

> Answers with NumPy/PyTorch. Full derivations for backprop, BatchNorm, Adam.

---

## MLP & Backpropagation

### 1. Perceptron / XOR (easy)
Single layer separates only linearly separable data; XOR needs a hidden layer.

### 2. Activations (detailed)
Without non-linearity, stacked linear layers collapse to one. ReLU cheap but can die; GELU/SiLU smooth, standard in transformers.
```python
relu = lambda x: np.maximum(0, x)
gelu = lambda x: 0.5*x*(1+np.tanh(0.797885*(x+0.044715*x**3)))
```

### 3. Universal approximation (tricky)
A wide 1-hidden-layer net can approximate any continuous function — but says nothing about width, trainability, or generalization.

### 4. Backprop through 2 layers (full derivation + code)
Forward `h=φ(W₁x)`, `ŷ=W₂h`. Backward:
```python
def backward(x, y, W1, W2):
    z1 = W1 @ x; h = np.maximum(0, z1); yhat = W2 @ h
    d2 = (yhat - y)                       # dL/dŷ (MSE)
    dW2 = np.outer(d2, h)
    d1 = (W2.T @ d2) * (z1 > 0)           # ReLU'
    dW1 = np.outer(d1, x)
    return dW1, dW2
```

### 5. Vanishing/exploding (detailed)
Product of Jacobian terms shrinks/blows through depth; saturating activations worsen it. Fix: init, residuals, norm, clipping, ReLU/GELU.

### 6. Init (detailed)
```python
# He init for ReLU keeps activation variance stable
W = np.random.randn(out_dim, in_dim) * np.sqrt(2.0 / in_dim)
```
Xavier for tanh/sigmoid; He for ReLU.

### 7. ReLU popularity (easy)
Cheap, non-saturating, sparse; dying units mitigated by Leaky/GELU.

---

## Optimization (Deep)

### 8. Update rules (code)
```python
# momentum
v = beta*v + g; theta -= lr*v
# rmsprop
s = beta*s + (1-beta)*g*g; theta -= lr*g/np.sqrt(s+1e-8)
# adam (see week 2)
```

### 9. Adam vs SGD (see Q8) — adaptive rates + momentum; can generalize slightly worse.

### 10. Gradient clipping (code)
```python
torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)
```

### 11. Warmup + cosine (see week 2).

---

## Training Mechanics

### 12. Overfitting (easy)
Train≪val loss, val rising. Fix: regularize, dropout, more data, early stop.

### 13. Dropout (code)
```python
def dropout(x, p=0.5, train=True):
    if not train: return x
    mask = (np.random.rand(*x.shape) > p) / (1 - p)  # inverted dropout
    return x * mask
```
Prevents co-adaptation ≈ ensembling subnetworks.

### 14. BatchNorm (full derivation + code)
```python
def batchnorm(x, gamma, beta, eps=1e-5):
    mu = x.mean(0); var = x.var(0)
    xhat = (x - mu) / np.sqrt(var + eps)
    return gamma * xhat + beta          # learned scale/shift
```
Stabilizes activation distributions, allows higher LR, smooths loss.

### 15. Why LayerNorm in transformers (detailed)
LayerNorm normalizes per token (across features), independent of batch size / seq length — unlike BatchNorm's unreliable batch stats for variable-length sequences.
```python
def layernorm(x, gamma, beta, eps=1e-5):
    mu = x.mean(-1, keepdims=True); var = x.var(-1, keepdims=True)
    return gamma * (x - mu)/np.sqrt(var + eps) + beta
```

### 16. Weight decay vs L2 (tricky)
Equivalent for plain SGD; AdamW decouples decay from the adaptive update.

### 17. Early stopping / augmentation (easy)
Stop when val stalls; augmentation expands data + enforces invariances.

---

## Scientific Thinking

### 18. Experiment design (detailed)
Hypothesis → change one variable → controls → metric + baseline → multiple seeds → watch confounders.

### 19. Ablation (easy)
Remove one component at a time to isolate its contribution.

---

## Coding

### 20. MLP on MNIST (PyTorch)
```python
import torch, torch.nn as nn
net = nn.Sequential(nn.Flatten(), nn.Linear(784,256), nn.BatchNorm1d(256),
                    nn.ReLU(), nn.Dropout(0.2), nn.Linear(256,10))
opt = torch.optim.Adam(net.parameters(), 1e-3)
loss_fn = nn.CrossEntropyLoss()
# for xb,yb in loader: opt.zero_grad(); loss_fn(net(xb),yb).backward(); opt.step()
```

### 21. BatchNorm/Dropout from scratch (see Q13, Q14).
### 22. Ablation: train 4 variants (±BN, ±Dropout), compare val curves.

---

## Whiteboard / Judgment

### 23. L1 vs L2 geometry (see week 3 Q3).
### 24. BatchNorm vs LayerNorm judgment: LayerNorm for transformers/variable batches; BatchNorm for CNNs/large fixed batches.
### 25. Silent gradient explosion: monitor grad/activation norms + NaNs; fix with clipping, lower LR, better init/norm, check data/mixed precision.
