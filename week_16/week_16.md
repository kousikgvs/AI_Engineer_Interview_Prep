# Week 16 — Post-Training: RLHF, DPO & Alignment

**Block:** F — RLHF + Agentic RL + Frontier Research  
**Goal:** Understand how raw pretrained models become aligned assistants.

---

## What You Will Learn

### Supervised Fine-Tuning (SFT)
- What SFT does: teaches the model format, instruction following, and style
- Dataset curation for SFT — quality matters more than quantity
- Instruction datasets: Alpaca, ShareGPT, OpenHermes
- Catastrophic forgetting and how to reduce it
- How to evaluate SFT models

### Reinforcement Learning from Human Feedback (RLHF)
- Why pretrained LLMs are not aligned by default
- Reward model training — Bradley-Terry model and pairwise comparisons
- PPO (Proximal Policy Optimization) applied to language models
- The InstructGPT pipeline: SFT → Reward Model → PPO
- KL penalty — why you need it and what happens without it
- Reward hacking — how models game the reward signal

### Direct Preference Optimization (DPO)
- The key insight behind DPO — removing the explicit reward model
- DPO derivation from the RLHF objective
- DPO vs RLHF — pros, cons, and when to use each
- SimPO and IPO — variants of DPO

### Group Relative Policy Optimization (GRPO)
- The key innovation: group-relative advantage estimation
- Why it is more stable than PPO for reasoning tasks
- DeepSeek's use of GRPO
- Implementation walkthrough

### Reinforcement Learning from Verifiable Rewards (RLVR)
- Using rule-based rewards instead of human labels
- Math and code as verifiable domains
- DeepSeek R1 approach — chain-of-thought as a learned behavior
- Why this may scale better than human feedback

### Constitutional AI & RLAIF
- Anthropic's Constitutional AI (CAI) approach
- RLAIF — using AI feedback instead of human feedback
- Self-critique and revision loops

---

## Project
- Train a small reward model on pairwise preference data
- Implement GRPO on a math reasoning dataset
- Fine-tune a model with DPO on a preference dataset
- Compare: base model vs SFT vs DPO on an alignment benchmark

---

## Papers to Read
- "Training Language Models to Follow Instructions with Human Feedback" — InstructGPT
- "Direct Preference Optimization" — Rafailov et al.
- "DeepSeek R1" — GRPO section in detail

---

## Failure Friday
- Analyze: A reward-hacked model — what unexpected behaviors emerge when the reward model gets gamed?

---

## Interview Questions This Week Prepares You For

<details>
<summary>"Derive DPO from first principles"</summary>

DPO reparameterizes the RLHF objective so the optimal policy relates to the reward in closed form, turning preference learning into a simple classification-style loss on (chosen, rejected) pairs — no separate reward model or RL loop.
</details>

<details>
<summary>"What is reward hacking and how do you detect it?"</summary>

The policy exploits reward-model flaws to score high without genuinely good behavior (verbose, sycophantic, formulaic). Detect via held-out human eval, KL-divergence spikes, and behavioral audits.
</details>

<details>
<summary>"Compare PPO vs GRPO — when would you choose each?"</summary>

PPO needs a learned value model (more memory/tuning, general). GRPO drops the critic and uses group-relative advantages — cheaper, more stable, and strong for RL-on-reasoning. Choose GRPO for reasoning/verifiable tasks, PPO when you need its generality.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"DPO or RLHF for this preference task?"</strong></summary>

**What I'd do:** DPO by default — simpler, stable, cheaper, no reward model or RL loop.
**Why:** it trains directly on preference pairs and works well.
**Tradeoff:** full RLHF/PPO can reach a higher ceiling with online reward shaping.
**Rejected:** PPO unless I need its flexibility and have the infra to tune it.
</details>
