# Week 23 — Deep Learning Extended: CV, GANs, VAEs & Diffusion

**Block:** J — Deep Learning & Multimodal Depth (Advanced Depth Track)
**Goal:** Go beyond transformers-for-text. Understand computer vision, generative models, and the architectures behind image/video AI — the depth research-adjacent and multimodal roles expect.

> Builds on Week 4 (DL foundations) and Week 6 (transformers). This week fills the vision and generative gaps the core sprint skips.

---

## What You Will Learn

### Robust Deep Learning (PyTorch, Production Style)
- PyTorch with clean OOP: Dataset, DataLoader, Model, Trainer classes
- Vanishing and exploding gradients revisited
- ReLU vs ELU vs GELU in practice
- Batch Normalization in depth
- Overfitting, dropout, and evaluation with torchmetrics
- TensorBoard for tracking training

### Convolutional Neural Networks & Computer Vision
- Convolutional layers — kernels, stride, padding, channels
- Pooling and receptive fields
- CNN architectures end-to-end (LeNet → ResNet intuition)
- Data augmentation for vision
- Image classification, object detection, background removal
- Transfer learning with pretrained vision backbones

### Sequence Modeling & Multi-IO Architectures
- RNNs, LSTMs, GRUs (applied, forecasting)
- Multi-input and multi-output architectures
- Loss weighting for multi-task models

### Deep Generative Models
- GANs — generator vs discriminator, adversarial training
- Implementing a GAN for handwritten digit generation
- Mode collapse and training instability
- Variational Autoencoders (VAEs) — the latent space and reconstruction
- Training a face-generation VAE (CelebA)
- Autoencoders for representation learning (MNIST autoencoder)

### Diffusion & Vision Transformers
- Diffusion models — forward/reverse process, denoising
- Inpainting and image-to-image
- Vision Transformers (ViT) — patches as tokens
- CLIP — connecting vision and language
- Zero-shot image/video classification with CLIP/CLAP

### Multimodal Model Foundations
- Vision-language models — how image and text embeddings align
- Document VQA and Visual Question Answering
- Where multimodal is heading (video understanding, world models)

---

## Project
- Build a CNN image classifier end-to-end with augmentation, BatchNorm, and torchmetrics evaluation
- Implement a GAN for digit generation and a VAE for face generation; compare their latent spaces
- Build a multi-input OCR model fusing an image stream and metadata stream
- Use CLIP for zero-shot image classification and measure accuracy vs a supervised baseline

---

## Papers / Reading
- Read: "Generative Adversarial Networks" — Goodfellow et al.
- Read: "Auto-Encoding Variational Bayes" (VAE) — Kingma & Welling
- Read: "Denoising Diffusion Probabilistic Models" — Ho et al. (concept level)
- Read: "Learning Transferable Visual Models From Natural Language Supervision" (CLIP) — Radford et al.

---

## Failure Friday
- Analyze: A GAN training run that collapsed to mode collapse — generating one image for every input. What signals it, and how do you stabilize training?

---

## Interview Questions This Week Prepares You For

<details>
<summary>"Explain the difference between a GAN and a VAE"</summary>

A GAN trains a generator against a discriminator (sharp samples, unstable training). A VAE learns an encoder/decoder over a probabilistic latent space (stable, principled, blurrier samples).
</details>

<details>
<summary>"How does a diffusion model generate an image from noise?"</summary>

Training adds noise to images and learns to remove it; generation starts from pure noise and iteratively denoises over many steps into a coherent image.
</details>

<details>
<summary>"What does CLIP actually learn, and why is it zero-shot capable?"</summary>

Contrastive training aligns image and text encoders in one shared space. You classify zero-shot by comparing an image to text prompts of candidate labels — no task-specific training needed.
</details>

<details>
<summary>"Why do CNNs use weight sharing and what does that buy you?"</summary>

The same kernel applies across all positions, giving translation invariance and far fewer parameters — better generalization and efficiency on images.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"GAN, VAE, or diffusion model for this image-generation use case?"</strong></summary>

**What I'd do:** diffusion for highest-quality controllable generation; VAE for a smooth latent/fast sampling; GAN for fast generation when you can manage instability.
**Why:** diffusion leads quality; VAEs are stable/structured.
**Tradeoff:** diffusion is slow at inference (many steps).
**Rejected:** GANs when mode collapse/instability is a risk and quality matters most.
</details>
