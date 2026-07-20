# Week 13 — Interview Questions: Agent Engineering

> Click a question (▸) to expand the answer.

---

## Agent Foundations

<details>
<summary>1. What is an agent (the PRAOR loop)?</summary>

A system that Perceives input, Reasons about it, Acts via tools, Observes the result, and Repeats — pursuing a goal over multiple steps rather than a single response.
</details>

<details>
<summary>2. Where do agents fail at each stage?</summary>

Perceive: bad/incomplete context. Reason: flawed planning. Act: wrong tool or bad arguments. Observe: misreading results. Repeat: compounding earlier errors and context loss over long loops.
</details>

<details>
<summary>3. Environment types — why do they matter?</summary>

Fully vs partially observable and deterministic vs stochastic environments change how much planning, memory, and error-handling the agent needs. Partial/stochastic ones require robustness and verification.
</details>

<details>
<summary>4. Why is tool use the fundamental primitive?</summary>

Tools let the model act on the world (read files, run code, search) and get grounded feedback, turning a text predictor into something that can accomplish tasks.
</details>

<details>
<summary>5. What causes compounding errors?</summary>

Each step conditions on prior (possibly wrong) steps, so small mistakes accumulate over a long trajectory. Mitigate with verification, critics, checkpoints, and short loops.
</details>

## Architectures

<details>
<summary>6. Single-agent vs multi-agent — when worth it?</summary>

Multi-agent helps when tasks decompose into specialties or need debate/review, but adds coordination overhead and failure surface. Prefer a single well-tooled agent unless separation clearly helps.
</details>

<details>
<summary>7. What is ReAct?</summary>

Interleaving Reasoning ("Thought") and Acting ("Action" → "Observation") so the model plans, acts, sees results, and adjusts — grounding reasoning in tool feedback.
</details>

<details>
<summary>8. What is a Reflexion agent?</summary>

Think → Act → Evaluate → Reflect → Retry: the agent critiques its own failed attempt and stores a reflection to do better on the next try.
</details>

<details>
<summary>9. Planner-Executor vs hierarchical vs critic vs debate?</summary>

Planner-Executor separates planning from doing. Hierarchical uses a manager delegating to workers. Critic adds a separate evaluator. Debate pits agents against each other to surface errors.
</details>

<details>
<summary>10. Canonical workflow patterns?</summary>

Prompt chaining (sequential steps), routing (pick a path), parallelization (fan-out/aggregate), orchestrator-workers (dynamic delegation), evaluator-optimizer (generate then critique/refine).
</details>

## Reliability & Long-Running Agents

<details>
<summary>11. What is HITL; when require approval?</summary>

Human-in-the-loop pauses for approval before risky/irreversible actions (deleting data, sending emails, spending money) — trading autonomy for safety.
</details>

<details>
<summary>12. What is durable execution; when needed?</summary>

The ability to survive crashes/restarts by persisting state and resuming (e.g., Temporal). Needed for long-running, multi-step agents where losing progress is costly.
</details>

<details>
<summary>13. Checkpointing and idempotency in agents?</summary>

Checkpointing saves state so you can resume. Idempotent actions can be retried safely without duplicate side effects — essential when steps may re-run after a failure.
</details>

<details>
<summary>14. How do audit trails and escalation build trust?</summary>

Logging every action/decision makes behavior explainable and reviewable; surfacing uncertainty and escalating to humans prevents silent bad actions — users trust systems they can inspect and control.
</details>

## System Design

<details>
<summary>15. Design a multi-agent code-review system; prevent cascading failures.</summary>

Specialized agents (analyzer, security, style) with a coordinator; isolate failures (timeouts, retries, circuit breakers), validate each agent's output, use a critic to catch errors, keep humans in the loop for merges, and log everything.
</details>

<details>
<summary>16. Design a coding agent with HITL and audit trail.</summary>

ReAct loop with tools (read/write files, run commands, search), HITL approval for destructive actions, durable execution for long tasks, full audit log, and a critic skill to review changes before applying.
</details>

## Coding

<details>
<summary>17. Build a ReAct agent with planner/executor/critic skills.</summary>

Loop: model emits a thought + tool call, you execute and return the observation; a planner decomposes the goal, an executor runs steps, a critic reviews outputs and triggers retries.
</details>

## Failure / Judgment

<details>
<summary>18. AutoGPT collapsed — analyze the compounding error.</summary>

Long open-ended loops with weak goals, no verification, and lost context accumulated errors and got stuck/looped. Fixes: bounded loops, explicit goals/plans, tool verification, critics, memory, and HITL.
</details>

<details>
<summary>19. "Multi-agent or single-agent architecture?"</summary>

**What I'd do:** single agent with good tools first; multi-agent only when the task clearly splits into specialties or needs review/debate.
**Why:** fewer moving parts, easier to debug.
**Tradeoff:** multi-agent adds coordination and failure surface.
**Rejected:** multi-agent for tasks a single tooled agent handles.
</details>
