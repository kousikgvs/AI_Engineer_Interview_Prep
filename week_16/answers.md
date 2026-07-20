# Week 16 — Answers with Code: Post-Training — RLHF, DPO & Alignment

> SFT, reward models, PPO, DPO with the key math and code.

---

## Supervised Fine-Tuning

### 1. What SFT teaches (easy)
Instruction-following + format/style from curated (prompt, good-response) pairs → raw predictor becomes assistant.

### 2. Quality over quantity (easy)
Small, high-quality, diverse, correct set beats large noisy set — curation teaches good habits.

### 3. Catastrophic forgetting (detailed)
Narrow fine-tuning erodes general skills. Mitigate: mix in general data, low LR, LoRA/PEFT, KL regularization to base.

---

## RLHF

### 4. Why not aligned by default (detailed)
Pretraining predicts web text → capability, not helpfulness/harmlessness/honesty. Alignment needs preference training.

### 5. Reward model training (detailed + code)
Bradley-Terry: preferred response should score higher.
```python
# loss = -log sigmoid(r(chosen) - r(rejected))
import torch.nn.functional as F
def rm_loss(r_chosen, r_rejected):
    return -F.logsigmoid(r_chosen - r_rejected).mean()
```

### 6. InstructGPT pipeline (detailed)
1) SFT on demonstrations. 2) Reward model on human rankings. 3) PPO policy optimization vs reward + KL penalty to SFT.

### 7. PPO objective (detailed)
Maximize reward while a KL penalty keeps the policy near the SFT model (prevents reward hacking / drift).
```python
# per-token: reward - beta * KL(policy || ref)
advantage = reward - value_baseline
ratio = torch.exp(logp_new - logp_old)
ppo = -torch.min(ratio * advantage,
                 torch.clamp(ratio, 1-eps, 1+eps) * advantage).mean()
```

### 8. Reward hacking (detailed)
Policy exploits reward-model flaws (verbose/sycophantic) → KL penalty, better reward data, adversarial eval.

---

## DPO & Alternatives

### 9. DPO vs RLHF (detailed + code)
DPO skips the reward model + RL loop; directly optimizes the policy on preference pairs with a closed-form loss.
```python
# beta controls deviation from reference
def dpo_loss(logp_chosen, logp_rejected, ref_chosen, ref_rejected, beta=0.1):
    chosen = beta * (logp_chosen - ref_chosen)
    rejected = beta * (logp_rejected - ref_rejected)
    return -F.logsigmoid(chosen - rejected).mean()
```
Simpler, stable, no reward model — dominant in practice.

### 10. Why DPO works (detailed)
The RLHF optimum has a closed form linking reward to log-prob ratios; DPO plugs that in so preference data trains the policy directly.

### 11. KTO / IPO / ORPO (easy)
KTO uses unpaired good/bad labels; IPO fixes DPO overfitting; ORPO folds preference into SFT (no reference model).

### 12. Constitutional AI / RLAIF (detailed)
Replace human labels with AI feedback guided by a set of principles ("constitution") → scalable alignment.

---

## Alignment Concepts

### 13. Helpful/harmless/honest (easy)
The three alignment axes; often in tension (helpful vs harmless) → balance via preference data.

### 14. Sycophancy (detailed)
Model agrees with the user to win reward → diversify raters, penalize in eval, reward correctness over agreement.

### 15. Over-refusal / alignment tax (easy)
Too much safety training hurts helpfulness/capability; balance with calibrated refusal data.

---

## Coding

### 16. Reward model loss (see Q5).
### 17. DPO loss (see Q9).
### 18. PPO clipped objective (see Q7).

---

## Judgment / Failure

### 19. Model became sycophantic: reward favored agreement → correctness-based rewards, diverse raters, adversarial eval.
### 20. RLHF unstable / reward hacking: weak reward model / no KL → stronger RM, KL penalty, or switch to DPO.
### 21. Over-refusing safe requests: alignment tax → add benign examples, calibrate refusal boundary.
