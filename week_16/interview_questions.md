# Week 16 — Interview Questions: Post-Training — RLHF, DPO & Alignment

> Sourced from research-engineer / alignment screens (OpenAI/Anthropic research). Expect derivations.

---

## Supervised Fine-Tuning
1. What does SFT actually teach a model (format, instruction-following, style)?
2. Why is quality more important than quantity for SFT data?
3. What is catastrophic forgetting and how do you mitigate it?

## RLHF
4. Why aren't pretrained LLMs aligned by default?
5. How is a reward model trained (Bradley-Terry, pairwise comparisons)?
6. Walk through the InstructGPT pipeline: SFT → RM → PPO.
7. Why do you need the KL penalty in PPO? What happens without it?
8. **What is reward hacking and how do you detect it?**
9. Why is PPO hard to tune for language models?

## DPO & Variants
10. **Derive DPO from the RLHF objective.** What is the key insight (removing the RM)?
11. **DPO vs RLHF — pros, cons, when to use each?**
12. What do SimPO and IPO change?

## GRPO & RLVR
13. What is the key innovation in GRPO (group-relative advantage)?
14. **Compare PPO vs GRPO — when would you choose each?**
15. What is RLVR (verifiable rewards)? Why do math/code make good domains?
16. How did DeepSeek R1 get chain-of-thought as a learned behavior?

## Constitutional AI / RLAIF
17. What is Constitutional AI? How does RLAIF differ from RLHF?

## Coding
18. Train a small reward model on pairwise preference data.
19. Implement GRPO on a math reasoning dataset; fine-tune with DPO.

## Failure / Judgment
20. A reward-hacked model — what unexpected behaviors emerge?
21. "DPO or RLHF for this preference task?" — defend it.
