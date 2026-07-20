# Week 17 — Interview Questions: Reasoning Models, Agentic RL & Research Thinking

> Click a question (▸) to expand the answer.

---

## Reasoning Models

<details>
<summary>1. Chain-of-thought as emergent behavior?</summary>

Large enough models spontaneously benefit from reasoning step-by-step; the ability to use intermediate steps appears with scale and training, not from an explicit reasoning module.
</details>

<details>
<summary>2. Trained CoT vs prompted CoT?</summary>

Prompted CoT elicits reasoning via the prompt ("think step by step"). Trained CoT bakes reasoning into the weights via RL/SFT on reasoning traces, so the model reasons by default and more reliably.
</details>

<details>
<summary>3. PRMs vs ORMs — when each?</summary>

Outcome reward models score only the final answer; process reward models score each reasoning step. PRMs give denser signal (catch a wrong step early) and improve reasoning, but are costlier to label.
</details>

<details>
<summary>4. What is o1-style reasoning (search over thought space)?</summary>

The model spends more inference compute exploring/evaluating multiple reasoning paths (search, self-evaluation) before answering — trading latency for accuracy on hard problems.
</details>

<details>
<summary>5. What is test-time compute scaling; why matter?</summary>

Spending more compute at inference (longer reasoning, more samples, search) improves accuracy on hard tasks — a new scaling axis beyond just bigger models/pretraining.
</details>

<details>
<summary>6. Majority voting vs best-of-N?</summary>

Majority voting takes the most common answer across samples. Best-of-N picks the highest-scoring sample by a verifier/reward model. Both spend inference compute to boost accuracy.
</details>

<details>
<summary>7. MCTS for language models?</summary>

Monte Carlo Tree Search explores a tree of reasoning steps, expanding promising branches and backing up value estimates — structured search over the thought space for planning/reasoning.
</details>

## Tool Use via RL

<details>
<summary>8. Train an agent to use tools via RL (not prompting)?</summary>

Let the agent take tool actions in an environment, reward successful task completion (verifiable outcomes), and update the policy so it learns when/how to call tools — tool use becomes a learned skill.
</details>

<details>
<summary>9. Why is code execution a good verifiable RL environment?</summary>

Running code gives an objective pass/fail (tests), so the reward is automatic and un-gameable — ideal RL signal without human labeling.
</details>

<details>
<summary>10. What did Toolformer do?</summary>

It taught a model to decide when to call APIs (calculator, search) in a self-supervised way — inserting tool calls where they improve next-token prediction.
</details>

## Agentic RL

<details>
<summary>11. Credit assignment over long trajectories?</summary>

Determining which of many earlier actions caused a late reward. It's hard because reward is sparse/delayed; techniques include process rewards, value functions, and shaping.
</details>

<details>
<summary>12. Exploration in agent RL (UCB, epsilon-greedy)?</summary>

Balancing trying new actions vs exploiting known-good ones. Epsilon-greedy acts randomly a fraction of the time; UCB favors uncertain-but-promising actions — adapted to LLM action spaces.
</details>

<details>
<summary>13. What is curriculum learning for agents?</summary>

Train on easy tasks first, then progressively harder ones, so the agent builds skills gradually instead of failing on hard tasks from the start.
</details>

<details>
<summary>14. Multi-agent RL — cooperation vs competition?</summary>

Multiple learning agents interact; cooperative setups share a goal (coordination), competitive ones oppose (self-play, which can drive capability). Both add non-stationarity since others also learn.
</details>

## Scientific Thinking

<details>
<summary>15. Design a rigorous ML experiment.</summary>

State a hypothesis, define independent/dependent variables, hold everything else fixed (controls), pick metrics and baselines, use multiple seeds and enough data for significance, and watch for confounders.
</details>

<details>
<summary>16. Ablation study; statistical significance?</summary>

Remove one component at a time to measure its contribution. Test significance with multiple seeds and a statistical test so improvements aren't just noise.
</details>

<details>
<summary>17. Replication crisis in ML; what it means for you?</summary>

Many published gains don't reproduce due to weak baselines, cherry-picking, or tuning leakage. Be skeptical, report seeds/variance, and reproduce before trusting a result.
</details>

<details>
<summary>18. Spot benchmark overfitting in a results table?</summary>

Watch for tuning on the test set, unfair baselines, single-seed results, cherry-picked metrics, and gains that vanish on held-out or harder benchmarks.
</details>

## Research Taste

<details>
<summary>19. Name field-changing papers and why they mattered.</summary>

Attention Is All You Need (architecture), Chinchilla (scaling laws), FlashAttention (systems), LoRA (efficient tuning), InstructGPT (alignment), DPO (simpler alignment), DeepSeek R1 (RL reasoning) — each shifted how the field builds models.
</details>

<details>
<summary>20. How to tell foundational papers from hype?</summary>

Foundational ones introduce durable ideas/abstractions, reproduce, and get built upon; hype papers over-claim on narrow benchmarks and don't generalize or replicate.
</details>

## Judgment / Design

<details>
<summary>21. Design a training setup to teach tool use via RL.</summary>

Build an environment with tools and verifiable tasks, define an outcome reward (task success), sample trajectories, and optimize the policy (e.g., GRPO); add a curriculum and process rewards to ease credit assignment.
</details>

<details>
<summary>22. "PRMs or ORMs for this reasoning task?"</summary>

**What I'd do:** PRMs when reasoning quality matters and you can label steps; ORMs when only the final answer is checkable and labeling steps is too costly.
**Why:** PRMs give denser, earlier signal.
**Tradeoff:** PRMs need expensive step-level labels.
**Rejected:** ORM-only when it lets bad reasoning reach correct answers by luck.
</details>
