# Week 13 — Interview Questions: Agent Engineering

> Sourced from agent-company screens (Cognition, Imbue, LangChain). They probe production reliability, not demos.

---

## Agent Foundations
1. What is an agent? Walk through the Perceive → Reason → Act → Observe (PRAOR) loop.
2. **Where do agents fail at each stage of the loop?**
3. Fully observable vs partial, deterministic vs stochastic environments — why does it matter?
4. Why is tool use the fundamental primitive of agents?
5. What causes compounding errors in multi-step agents?

## Architectures
6. Single-agent vs multi-agent — when is multi-agent actually worth the complexity?
7. What is ReAct? How does interleaving thought and action help?
8. What is a Reflexion agent (Think → Act → Evaluate → Reflect → Retry)?
9. Planner-Executor vs hierarchical (manager/worker) vs critic vs debate — compare.
10. Canonical workflow patterns: prompt chaining, routing, parallelization, orchestrator-workers, evaluator-optimizer — give an example of each.

## Reliability & Long-Running Agents
11. What is Human-in-the-Loop (HITL)? When do you require approval?
12. What is durable execution and when do you need it (Temporal)?
13. What is checkpointing agent state? Why is idempotency important in agent actions?
14. How do audit trails and escalation design build user trust?

## System Design
15. **Design a multi-agent system for code review — how do you prevent cascading failures?**
16. Design a coding agent (file I/O, bash, web search) with HITL and audit trail.

## Coding
17. Build a ReAct agent with tools and a /planner-/executor-/critic skill system.

## Failure / Judgment
18. **AutoGPT collapsed in production — analyze the compounding-error failure.**
19. "Multi-agent or single-agent architecture for this task?" — defend it.
