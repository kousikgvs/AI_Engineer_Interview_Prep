# Week 2 — Answers with Code: Linear Algebra, Optimization & Calculus

> Detailed answers with code (NumPy/PyTorch). Derivations for the hard ones.

---

## Linear Algebra

### 1. Matrix multiply = linear transformation (easy)
Columns show where basis vectors land.
```python
import numpy as np
A = np.array([[0, -1], [1, 0]])   # 90° rotation
print(A @ np.array([1, 0]))       # -> [0, 1]
```

### 2. Rank & low rank (detailed — LoRA basis)
Rank = # independent rows/cols. Low rank ⇒ compressible; LoRA approximates a weight update ΔW ≈ B·A with small r.
```python
M = np.outer([1,2,3], [4,5,6])    # rank-1 (any row is a multiple of another)
print(np.linalg.matrix_rank(M))   # 1
```

### 3. Eigen (code)
```python
vals, vecs = np.linalg.eig(np.array([[2, 0], [0, 3]]))
# eigenvector direction unchanged; eigenvalue = scaling
```

### 4. SVD (detailed)
`A = U Σ Vᵀ` = rotate, scale by singular values, rotate.
```python
U, S, Vt = np.linalg.svd(np.random.rand(5, 3), full_matrices=False)
# low-rank approx: keep top-k singular values
k = 2
A_k = U[:, :k] @ np.diag(S[:k]) @ Vt[:k]
```

### 5. PCA from scratch (detailed)
```python
def pca(X, k):
    Xc = X - X.mean(0)
    U, S, Vt = np.linalg.svd(Xc, full_matrices=False)
    return Xc @ Vt[:k].T           # top-k principal components
```
PCA directions = top eigenvectors of the covariance = top right singular vectors.

### 6. Cosine similarity (detailed — retrieval)
```python
def cosine(a, b):
    return a @ b / (np.linalg.norm(a) * np.linalg.norm(b))
```
Measures angle (direction), ignoring magnitude — right for embeddings.

### 7. Invertibility/determinant (easy)
Invertible iff full rank / nonzero det; det = signed volume scaling.

### 8. Dot vs outer (easy)
`a·b` → scalar; `np.outer(a,b)` → rank-1 matrix.

---

## Calculus & Autodiff

### 9. Chain rule = backprop engine (detailed)
`d/dx f(g(x)) = f'(g(x))·g'(x)`; backprop chains local grads output→input.

### 10. Jacobian vs Hessian
Jacobian = first partials (vector functions); Hessian = second partials (curvature).

### 11. Computational graph (easy)
DAG of ops; autodiff records forward, traverses backward.

### 12–13. Autodiff (detailed)
```python
import torch
x = torch.tensor(3.0, requires_grad=True)
y = x**2 + 2*x
y.backward()
print(x.grad)   # dy/dx = 2x+2 = 8
```
Reverse-mode is cheap when outputs≪inputs (one loss, many params) — that's deep learning.

---

## Optimization

### 14. Gradient descent (derivation + code)
Step opposite the gradient (steepest increase): `θ ← θ − η∇L`.
```python
def gd(grad, theta, lr=0.1, steps=100):
    for _ in range(steps):
        theta = theta - lr * grad(theta)
    return theta
```

### 15. Batch/SGD/minibatch (easy)
All data (stable/slow) vs one sample (noisy/fast) vs minibatch (balanced default).

### 16. Warmup + cosine decay
```python
import math
def lr_at(step, warmup, total, base=1e-3):
    if step < warmup:
        return base * step / warmup                    # linear warmup
    p = (step - warmup) / (total - warmup)
    return 0.5 * base * (1 + math.cos(math.pi * p))     # cosine decay
```

### 17. Saddle points (detailed)
In high dimensions, most critical points are saddles (up in some dirs, down in others), not bad minima. Momentum/noise escape them.

### 18. Adam (derivation + code)
```python
# m,v are running 1st/2nd moment estimates; bias-corrected
m = beta1*m + (1-beta1)*g
v = beta2*v + (1-beta2)*g*g
mhat = m/(1-beta1**t); vhat = v/(1-beta2**t)
theta -= lr * mhat / (np.sqrt(vhat) + 1e-8)
```
Adapts per-parameter step sizes (momentum + RMSProp). Downside: can generalize slightly worse (use AdamW).

### 19. Momentum (easy)
Moving average of gradients → smooths noise, accelerates consistent directions.

### 20. Vanishing/exploding (detailed)
Repeated Jacobian multiplication shrinks/blows gradients. Fix: He/Xavier init, residuals, normalization, gradient clipping, ReLU/GELU.

---

## Applied

### 21. Fermi GPU hours
`6·params·tokens / (GPU FLOPs × util)`.

### 22. $5k evals vs compute
Evals de-risk quality (compounding value); usually invest there first unless capacity-bound.

---

## Coding

### 23. GD on 2D surface
```python
f = lambda p: p[0]**2 + p[1]**2
grad = lambda p: np.array([2*p[0], 2*p[1]])
p = np.array([3.0, 4.0])
for _ in range(50): p -= 0.1 * grad(p)   # -> ~[0,0]
```

### 24. Micrograd-style autograd (detailed)
```python
class Value:
    def __init__(self, data, _children=()):
        self.data = data; self.grad = 0.0
        self._back = lambda: None; self._prev = set(_children)
    def __mul__(self, o):
        out = Value(self.data * o.data, (self, o))
        def _back():
            self.grad += o.data * out.grad
            o.grad += self.data * out.grad
        out._back = _back; return out
    def backward(self):
        topo, seen = [], set()
        def build(v):
            if v not in seen:
                seen.add(v)
                for c in v._prev: build(c)
                topo.append(v)
        build(self); self.grad = 1.0
        for v in reversed(topo): v._back()
```

### 25. SGD vs Adam vs RMSProp
Run each from the same start on the same loss and plot paths; adaptive ones navigate ravines faster.

---

## Whiteboard

### 26. Backprop from first principles (see Q9, Q24).
### 27. SVD uses (see Q4): PCA, low-rank compression, recommender factorization, pseudo-inverse.
### 28. Adam vs SGD judgment: Adam for speed/sparse grads; SGD+momentum for best generalization on big runs.
