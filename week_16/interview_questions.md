# Week 16 — Interview Questions: Post-Training — RLHF, DPO & Alignment

> Click a question (▸) to expand the answer.

---

## Supervised Fine-Tuning

<details>
<summary>1. What does SFT teach a model?</summary>

To follow instructions and produce the desired format/style by training on curated (prompt, good-response) pairs — turning a raw next-token predictor into a helpful assistant.
</details>

<details>
<summary>2. Why quality over quantity for SFT data?</summary>

A small set of high-quality, diverse, correct examples teaches good behavior; noisy/low-quality data teaches bad habits. Curation beats volume.
</details>

<details>
<summary>3. What is catastrophic forgetting; mitigation?</summary>

Fine-tuning on a narrow task erodes general capabilities. Mitigate with mixed data (include general examples), low learning rates, PEFT/LoRA (fewer weights changed), and regularization toward the base model.
</details>

## RLHF

<details>
<summary>4. Why aren't pretrained LLMs aligned by default?</summary>

Pretraining only predicts next tokens from web text — it learns capability, not helpfulness/harmlessness/honesty. Alignment requires additional training on human preferences.
</details>

<details>
<summary>5. How is a reward model trained?</summary>

Humans rank pairs of responses; a reward model learns to score responses so preferred ones get higher reward (Bradley-Terry model on pairwise comparisons).
</details>

<details>
<summary>6. Walk through the InstructGPT pipeline.</summary>

1) SFT on demonstrations. 2) Train a reward model on human preference rankings. 3) Optimize the policy with PPO against the reward model, with a KL penalty to stay near the SFT model.
</details>

<details>
<summary>7. Why the KL penalty in PPO?</summary>

It keeps the policy from drifting too far from the reference model, preventing it from over-optimizing/gaming the reward model and losing fluency/capability.
</details>

<details>
<summary>8. What is reward hacking; detect it?</summary>

The policy exploits flaws in the reward model to get high reward without genuinely good behavior (verbose, sycophantic, formulaic answers). Detect via held-out human eval, KL divergence spikes, and behavioral audits.
</details>

<details>
<summary>9. Why is PPO hard to tune for LMs?</summary>

It's a multi-model RL loop (policy, reference, reward, value) that's memory-heavy, sensitive to hyperparameters, and unstable — which motivated simpler methods like DPO.
</details>

## DPO & Variants

<details>
<summary>10. Derive DPO — the key insight.</summary>

DPO reparameterizes the RLHF objective so the optimal policy relates to the reward in closed form, letting you train directly on preference pairs with a simple classification-style loss — no separate reward model or RL loop.
</details>

<details>
<summary>11. DPO vs RLHF — when each?</summary>

DPO is simpler, stable, and cheaper (no reward model/PPO) — great default. Full RLHF/PPO can reach higher ceilings and supports online/iterative reward shaping when you have the infrastructure.
</details>

<details>
<summary>12. What do SimPO and IPO change?</summary>

Variants that adjust DPO's objective — e.g., SimPO removes the reference model and uses length-normalized reward; IPO addresses DPO's overfitting to deterministic preferences.
</details>

## GRPO & RLVR

<details>
<summary>13. Key innovation in GRPO?</summary>

Group Relative Policy Optimization drops the value/critic model and estimates advantage from the relative rewards of a group of sampled responses to the same prompt — simpler and more stable for reasoning.
</details>

<details>
<summary>14. Compare PPO vs GRPO.</summary>

PPO needs a learned value function (extra model, memory, tuning). GRPO uses group-relative baselines instead — cheaper and more stable, and it shone for RL-on-reasoning (DeepSeek). PPO is more general.
</details>

<details>
<summary>15. What is RLVR; why math/code?</summary>

Reinforcement Learning from Verifiable Rewards uses automatic checkers (unit tests, math answer verification) instead of a learned reward model, so the signal is objective and un-gameable — ideal where correctness is checkable.
</details>

<details>
<summary>16. How did DeepSeek R1 get CoT as learned behavior?</summary>

By rewarding correct final answers on verifiable tasks via RL (GRPO), the model learned to produce long chains of thought on its own because reasoning improved reward — reasoning emerged from RL, not just prompting.
</details>

## Constitutional AI / RLAIF

<details>
<summary>17. Constitutional AI; RLAIF vs RLHF?</summary>

Constitutional AI uses a set of principles ("constitution") for the model to critique and revise its own outputs. RLAIF replaces human preference labels with AI-generated preferences — scaling alignment with less human labeling.
</details>

## Coding

<details>
<summary>18. Train a reward model on pairwise preferences.</summary>

Feed pairs (chosen, rejected) through the model, compute a scalar score for each, and train with a loss that pushes chosen scores above rejected (log-sigmoid of the score difference).
</details>

<details>
<summary>19. Implement GRPO / fine-tune with DPO.</summary>

GRPO: sample a group of responses per prompt, score them (verifier), compute group-relative advantages, and update the policy. DPO: minimize the preference loss on (chosen, rejected) pairs against a frozen reference.
</details>

## Failure / Judgment

<details>
<summary>20. A reward-hacked model — what emerges?</summary>

Sycophancy, verbosity, formulaic hedging, or gaming the format — high reward-model score but poor real quality. Catch with human eval and by penalizing KL drift.
</details>

<details>
<summary>21. "DPO or RLHF for this preference task?"</summary>

**What I'd do:** DPO by default — simpler, stable, cheaper, no reward model or RL loop.
**Why:** it trains directly on preference pairs and works well.
**Tradeoff:** full RLHF/PPO can reach a higher ceiling with online reward shaping.
**Rejected:** PPO unless I need its flexibility and have the infra to tune it.
</details>
