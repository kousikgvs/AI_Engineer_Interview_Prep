# Week 23 — Answers with Code: Deep Learning Extended — CV, GANs, VAEs & Diffusion

> CNNs and generative vision models with the core math and code.

---

## CNNs

### 1. Convolutional layer (detailed + code)
A learnable kernel slides over the input computing local weighted sums → feature maps.
```python
import torch.nn as nn
conv = nn.Conv2d(in_channels=3, out_channels=64, kernel_size=3, stride=1, padding=1)
# out_size = (in + 2*pad - kernel)/stride + 1
```

### 2. Weight sharing (detailed)
Same kernel at every position → translation invariance + far fewer params than dense → better generalization/efficiency.

### 3. Pooling & receptive field (detailed)
Pooling downsamples for invariance/efficiency. Receptive field = input region influencing one output; grows with depth → deep layers see more context.

### 4. ResNet residuals (detailed)
Deep plain nets degrade (vanishing gradients). Skip connections learn residuals `y = F(x) + x` → very deep nets train.

### 5. ReLU/ELU/GELU (easy)
ReLU: fast, can die. ELU: smooth negatives. GELU: smooth gating, transformer standard.

### 6. Transfer learning (code)
```python
model = torchvision.models.resnet50(pretrained=True)
for p in model.parameters(): p.requires_grad = False   # freeze backbone
model.fc = nn.Linear(2048, num_classes)                # train new head
```

---

## VAEs

### 7. VAE idea (detailed)
Encoder → distribution over latent z; decoder reconstructs. Trains an interpretable, samplable latent space.

### 8. VAE loss / ELBO (detailed + code)
Reconstruction + KL(q(z|x) ‖ N(0,I)).
```python
def vae_loss(x, x_hat, mu, logvar):
    recon = F.mse_loss(x_hat, x, reduction="sum")
    kl = -0.5 * torch.sum(1 + logvar - mu**2 - logvar.exp())
    return recon + kl
```

### 9. Reparameterization trick (detailed + code)
Sample via `z = mu + sigma*eps` so gradients flow through mu/sigma.
```python
def reparam(mu, logvar):
    std = torch.exp(0.5 * logvar)
    return mu + std * torch.randn_like(std)
```

---

## GANs

### 10. GAN idea (detailed)
Generator makes fakes; discriminator distinguishes real/fake; adversarial minimax → generator learns the data distribution.
```python
# d_loss: maximize log D(real) + log(1 - D(G(z)))
# g_loss: maximize log D(G(z))  (non-saturating)
```

### 11. Mode collapse (detailed)
Generator produces limited variety. Mitigate: minibatch discrimination, feature matching, WGAN-GP, spectral norm.

### 12. WGAN (easy)
Wasserstein distance + gradient penalty → more stable training, meaningful loss.

---

## Diffusion

### 13. Diffusion idea (detailed)
Forward: gradually add Gaussian noise. Reverse: train a model to denoise step by step → generate from noise.

### 14. Training objective (code)
Predict the noise added at a random timestep.
```python
def diffusion_loss(model, x0, t, noise):
    xt = sqrt_acp[t] * x0 + sqrt_1macp[t] * noise   # noised sample
    pred = model(xt, t)
    return F.mse_loss(pred, noise)                   # predict the noise
```

### 15. Sampling / DDPM vs DDIM (detailed)
DDPM: many stochastic denoising steps. DDIM: deterministic, fewer steps, faster.

### 16. Latent diffusion / Stable Diffusion (detailed)
Diffuse in a VAE latent space (not pixels) → much cheaper; text conditioning via cross-attention on CLIP embeddings.

### 17. Classifier-free guidance (detailed)
Interpolate conditional/unconditional predictions: `pred = uncond + s*(cond - uncond)` → stronger prompt adherence.

---

## Coding

### 18. VAE loss + reparam (see Q8/Q9).
### 19. Diffusion training step (see Q14).
### 20. Transfer learning head (see Q6).

---

## Judgment / Failure

### 21. GAN won't converge / mode collapse: unstable minimax → WGAN-GP, spectral norm, balanced training.
### 22. Diffusion too slow: many steps → DDIM/fewer steps, latent diffusion, distillation.
### 23. Overfitting small image set: use pretrained backbone + augmentation + freeze layers.
