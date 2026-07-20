# Week 17 — Reasoning Models, Agentic RL & Research Thinking

**Block:** F — RLHF + Agentic RL + Frontier Research  
**Goal:** Frontier knowledge. Train reasoning models. Understand what is coming next.

---

## What You Will Learn

### Reasoning Models
- Chain-of-Thought as emergent behavior in large models
- Trained CoT vs prompted CoT — the key difference
- Process reward models (PRMs) vs outcome reward models (ORMs)
- o1-style reasoning — search over the thought space
- Test-time compute scaling — why spending more compute at inference helps
- Majority voting and best-of-N sampling
- Monte Carlo Tree Search (MCTS) for language models

### Tool Use via RL
- Training agents to use tools through reinforcement learning
- Tool use as a learnable skill — not just a prompted behavior
- Code execution as a verifiable environment for RL
- Toolformer — the original tool use via self-supervised learning paper

### Agentic RL
- Multi-step RL in agent environments
- Credit assignment over long trajectories
- Exploration in agent RL — UCB, epsilon-greedy adapted for LLMs
- Curriculum learning for agents
- Multi-agent RL — cooperation and competition

### Scientific Thinking (Full Block)
- Experiment design for ML research: hypothesis, independent variable, dependent variable, controls
- Ablation studies — isolating one variable at a time
- Statistical significance in ML experiments
- Reading results tables critically
- The replication crisis in ML — what it means for your work

### Research Taste (Advanced)
Papers that actually changed the field:
- Attention Is All You Need — architecture revolution
- Chinchilla — scaling law revision
- FlashAttention — systems innovation
- LoRA — efficiency revolution
- InstructGPT — alignment breakthrough
- DPO — simplifying alignment
- DeepSeek R1 — reasoning via RL

How to tell which papers actually matter vs. hype.

### Frontier Research Areas
- World models — predicting future states of the environment
- Multimodal models — vision-language, video understanding
- Long context — 1M token models and their applications
- Agent foundations — formal theories of agency
- Mechanistic interpretability — understanding what models actually compute

---

## Project — Frontier Labs
You must:
1. Find a real problem that no tutorial addresses
2. Read 10 relevant papers
3. Propose a new abstraction or approach
4. Build a prototype
5. Write a research memo
6. Present findings with a live demo

**Project options:**
- Train a math reasoning model using GRPO — implement from scratch
- Build a process reward model for code generation
- Implement a simplified o1-style reasoning loop with tree search
- Create a novel evaluation methodology for multi-step agents

---

## Papers to Read
- "DeepSeek R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning"
- "Scaling LLM Test-Time Compute Optimally Outperforms o1" (concept level)
- "Toolformer: Language Models Can Teach Themselves to Use Tools" — Schick et al.

---

## Failure Friday
- Analyze: A reasoning model that developed reward hacking in chain-of-thought — generating plausible-sounding but wrong reasoning steps

---

## Interview Questions This Week Prepares You For
- "What is test-time compute scaling and why does it matter?"
- "Explain process reward models vs outcome reward models"
- "How would you design a training setup to teach an agent to use tools via RL?"

---

## Engineering Judgment Question
**"PRMs or ORMs for this reasoning task?"**  
Write your answer covering: what you would do, why, what tradeoff you are making, and what alternative you rejected.
