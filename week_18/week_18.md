# Week 18 — Capstone: Production AI System + Hiring Sprint

**Block:** G — Production Capstone + Hiring Sprint  
**Goal:** Ship a real system. Get hired.

---

## What You Will Learn

### AI-Native Systems Design
- Architecture Decision Records (ADRs) — how to document decisions and trade-offs
- System design for AI products — unique considerations vs traditional software systems
- AI product design — onboarding, trust, and explainability
- Capstone system design principles

### Product Engineering
- Customer discovery — problem interviews and Jobs to Be Done (JTBD)
- Product metrics — activation, retention, engagement, NPS
- Unit economics for AI products — cost per query, margin
- A/B testing AI features — measurement and statistical significance
- Feature prioritization — ICE, RICE, impact vs effort

### AI Product Intuition
- Why Cursor won — UX, low latency, product-led trust
- Why Perplexity won — search UX, citation trust, speed
- Why Claude Code won — ergonomics, safety, tool use depth
- What would you do differently? Building taste through critique

### Distribution
- Technical writing — blog posts that demonstrate depth
- Building in public — GitHub, X, LinkedIn as proof-of-work portfolios
- Demos that sell — the art of the technical demo
- OSS contribution as a hiring signal

### Full-Stack AI Development
- End-to-end product: Frontend + Backend + DB + Agent + Deployment
- Rapid AI UIs for demos: Streamlit and Gradio; production frontends with Next.js
- AI coding tools at full speed: Claude Code + Cursor + Codex as daily tools
- Reviewing and debugging AI-generated code
- Architecture with AI — prompting for system design decisions

---

## Capstone Tracks (Pick One)

### Track A — AI SWE
- PR Review Agent
- Full repo analysis, multi-file changes, diff comprehension, comment generation

### Track B — Forward Deployed Engineer (FDE)
- Domain vertical: Hospital / Legal / Compliance / Supply Chain
- Hospital Agent: patient intake, diagnosis support, document processing
- Legal Agent: contract review, clause extraction, compliance checking

### Track C — Applied AI
- Research Agent: paper summarization, hypothesis generation, experiment tracking
- Sales Agent: lead research, outreach personalization, CRM integration
- Knowledge Agent: internal knowledge base with multi-hop reasoning

### Track D — AI Infrastructure
- Evaluation Platform: golden datasets, LLM-as-judge, regression testing, dashboards
- Agent Observability Platform: tracing, cost tracking, failure analysis
- LLM Gateway: routing, caching, cost optimization, fallbacks

---

## Required Deliverables (All Tracks)
- ADRs documenting every major technical decision
- Full system design document
- Eval harness with golden dataset and LLM-as-judge
- Deployed production system (not localhost)
- Monitoring and cost dashboard
- Postmortem: what failed during development and why
- Live demo (20 min: problem → solution → architecture → demo → eval results)
- Public write-up: LinkedIn post, technical blog, or X thread

---

## Hiring Sprint
- Resume review + ATS optimization
- Portfolio audit: GitHub, deployed links, write-ups
- Interview prep: system design, ML fundamentals, coding
- Mock interviews (peer + mentor)
- Cold outreach strategy using proof-of-work
- Submit a PR to at least one OSS project (LangGraph, vLLM, LiteLLM, or OpenHands)

---

## Paper to Read
- "LLM-as-a-Judge" — Zheng et al. (for your capstone eval harness)

---

## Failure Friday (Final)
- Each student analyzes one failure from their own capstone build — postmortem format:
  1. What happened?
  2. What failed technically?
  3. What monitoring would have caught it?
  4. What would you do differently?

---

## Interview Questions This Week Prepares You For

<details>
<summary>"Walk me through your capstone system design"</summary>

Present the problem, architecture (components, data flow, model choices), key tradeoffs (ADRs), how you evaluated it, what failed, and the live deployment — narrating decisions and why.
</details>

<details>
<summary>"How did you evaluate your system?"</summary>

Golden dataset + LLM-as-judge, task-specific metrics, regression tests, and production monitoring (quality, cost, latency). Show numbers and how you caught regressions.
</details>

<details>
<summary>"What failed? What would you do differently?"</summary>

Give an honest postmortem: what broke, why, what monitoring should have caught it, and the concrete fix — demonstrating engineering maturity.
</details>

<details>
<summary>"Show me a production deployment"</summary>

A live URL (not localhost) on real infrastructure with monitoring/cost dashboards and CI/CD — proving you can ship and operate, not just prototype.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"Build vs buy: host your own model or use the API?"</strong></summary>

**What I'd do:** start with an API for speed/quality/low ops; self-host at scale for cost, privacy, or control.
**Why:** APIs let you ship and validate fast.
**Tradeoff:** APIs cost more per token and give less control; self-hosting adds ops burden.
**Rejected:** self-hosting early before volume/privacy justifies it.
</details>

---

## What Good Looks Like at Graduation

**On a whiteboard:**
- Derive attention from scratch
- Explain KV cache, RoPE, SwiGLU without notes
- Design a production RAG system with proper retrieval
- Architect a multi-agent system and explain failure modes
- Explain RLHF → DPO → GRPO progression and tradeoffs
- Roofline analysis for a transformer workload
- CAP theorem applied to a real AI system

**In code:**
- Ship a full-stack AI product solo (frontend + backend + DB + agent + deployment)
- Build and evaluate a RAG pipeline that outperforms a naive baseline
- Run a LoRA fine-tune and evaluate it properly
- Write a working GRPO training loop on a math dataset
- Deploy and monitor an AI system in production

**As a portfolio:**
- 18 weeks of LinkedIn/X posts documenting learning
- Deployed capstone with a live demo link
- At least one merged OSS PR
- 3–5 technical blog posts or architecture write-ups
- GitHub with clean, documented projects
