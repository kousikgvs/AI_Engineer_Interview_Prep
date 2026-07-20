# Week 13 — Agent Engineering

**Block:** D — LLM Engineering + RAG + Agents  
**Goal:** Build production-grade AI agents, not toy demos.

---

## What You Will Learn

### Agent Foundations
- What is an agent? (Perceive → Reason → Act → Observe → Repeat)
- The PRAOR loop in detail
- Environment types — fully observable vs partial, deterministic vs stochastic
- Tool use as the fundamental building block of agents
- Why agents fail: compounding errors, tool misuse, context loss

### Agent Architectures
- Single Agent — one model, one loop
- ReAct (Reasoning + Acting) — interleaving thought and action
- Reflexion agents — Think → Act → Evaluate → Reflect → Retry
- Planner-Executor — separate models for planning and execution
- Multi-agent systems — why and when to use them
- Hierarchical agents — manager/worker patterns
- Critic agents — a separate model that evaluates the main agent's work
- Debate frameworks — adversarial agent pairs

> Framework note: you will implement these with LangGraph, CrewAI, AutoGen, and MCP tool connections. Full framework mastery (LangGraph state, MCP servers, Mem0, graph memory) is in the Advanced Depth Track — [Week 24](../week_24/week_24.md).

### Dynamic Workflows
- Static vs dynamic task decomposition
- Canonical workflow patterns: prompt chaining, routing, parallelization, orchestrator-workers, evaluator-optimizer
- Conditional branching in agent loops
- Parallel agent execution
- Agent handoff patterns
- Event-driven agent triggers

### Human-in-the-Loop (HITL)
- When to require human approval before acting
- Interrupt and resume patterns
- Audit trails for agent actions
- Trust and escalation design
- UX for AI products — how users actually interact with agents

### Long-Running Agents
- Durable execution — surviving process restarts
- Temporal workflows — durable task execution engine
- Checkpointing agent state
- Idempotency in agent actions

### Skills Architecture
- Skill abstraction — composable, reusable agent capabilities
- /detective — information gathering skill
- /scout — research and exploration skill
- /council — multi-perspective reasoning skill
- Building your own skill library

### Human-Computer Interaction for AI
- How users build and lose trust in AI systems
- Explainability — can you explain what the agent did and why?
- Escalation design — when and how to surface uncertainty
- Human feedback loops — RLHF at the product level
- Studying what makes Cursor, Claude Code, and Perplexity trusted

---

## Project — Build a Coding Agent
Build "Tern Coding Agent" with:
- ReAct loop with tool use (file read/write, bash execution, web search)
- HITL approval for destructive actions
- Durable execution with Temporal
- Full audit trail
- Skill system: /planner, /executor, /critic

---

## Paper to Read
- "ReAct: Synergizing Reasoning and Acting in Language Models" — Yao et al.

---

## Failure Friday
- Analyze: AutoGPT — why it collapsed in production; compounding error analysis

---

## Interview Questions This Week Prepares You For
- "Design a multi-agent system for code review — how do you prevent cascading failures?"
- "What is durable execution and when do you need it?"
- "Walk me through the PRAOR loop and where agents fail at each stage"

---

## Engineering Judgment Question
**"Multi-agent or single-agent architecture for this task?"**  
Write your answer covering: what you would do, why, what tradeoff you are making, and what alternative you rejected.
