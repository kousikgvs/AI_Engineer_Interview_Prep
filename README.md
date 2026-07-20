# AI Engineer Interview Prep — Full Curriculum

A production-focused, 26-week path to becoming an **AI-native engineer** who can walk into OpenAI, Anthropic, Palantir FDE, Cursor, Cognition, Harvey, or any SF AI startup and be immediately useful.

**Philosophy:** Distribution over skill. Proof of work over credentials. Systems + Product + Reasoning + Execution — not just models.

This curriculum merges five sources into one coherent progression:

1. **AI Placement Sprint** — the 18-week backbone (math → transformers → systems → RAG → agents → RLHF → capstone)
2. **AWS services track** — cloud infrastructure for production
3. **AI Engineer roadmap + MLOps / LLMOps / AIOps stacks** (`topics.md`)
4. **AlgoCamp 6-month bootcamp** — Python foundations, deep learning depth, frameworks, voice/multimodal (`curriculum.md`)
5. **Vizuara Minor in Generative AI** — generative-modeling first principles + AI ethics/responsible AI

---

## How the Curriculum Works

The program runs from **Week 0 to Week 25**, organized into 11 blocks (A–K). Weeks 1–18 are the core sprint; Week 0 is a pre-sprint software foundation; Weeks 19–25 are advanced depth tracks (cloud, MLOps/LLMOps, deep-learning/multimodal, and responsible AI) that can be taken after Week 18 or interleaved with the relevant core blocks.

Every week follows the **same repeatable structure**: *What You Will Learn* → a hands-on *Project* → a *Paper to read* → a *Failure Friday* case study → *Interview Questions this week prepares you for* → an *Engineering Judgment* decision prompt. This keeps each week theory-light and shipping-heavy, and it maps every topic directly to something an interviewer will actually ask.

Five **horizontal tracks** run across all weeks in parallel: an **AI Coding Systems** track (Claude Code, Cursor, Codex, Gemini CLI, OpenHands as daily default tools), a **Startup Operator** track (ship weekly, talk to a user weekly, write weekly, contribute to OSS monthly), a weekly **Paper Club** (read → critique → reproduce → present), **Failure Friday** (analyze one real AI failure per week), and **Engineering Judgment** (answer four written decision questions every week). Together they build the distribution, communication, and taste that top companies over-index on.

---

## Curriculum Map

| Week | Block | Topic |
|------|-------|-------|
| [Week 0](week_00/week_00.md) | Foundation | Python, SQL & Developer Foundations |
| [Week 1](week_01/week_01.md) | A — Math + ML Foundations | Probability, Statistics & Information Theory |
| [Week 2](week_02/week_02.md) | A — Math + ML Foundations | Linear Algebra, Optimization & Calculus |
| [Week 3](week_03/week_03.md) | A — Math + ML Foundations | Classical ML & Engineering Judgment |
| [Week 4](week_04/week_04.md) | A — Math + ML Foundations | Deep Learning Foundations |
| [Week 5](week_05/week_05.md) | B — Transformers + LLM Internals | Sequence Models & the Road to Transformers |
| [Week 6](week_06/week_06.md) | B — Transformers + LLM Internals | Transformer Architecture Deep Dive |
| [Week 7](week_07/week_07.md) | B — Transformers + LLM Internals | Inference, Quantization & Fine-Tuning |
| [Week 8](week_08/week_08.md) | C — Systems + Backend + Data | GPU Architecture & AI Systems |
| [Week 9](week_09/week_09.md) | C — Systems + Backend + Data | Backend Engineering for AI Products |
| [Week 10](week_10/week_10.md) | C — Systems + Backend + Data | Data Engineering |
| [Week 11](week_11/week_11.md) | D — LLM Engineering + RAG + Agents | LLM Engineering & Context Engineering |
| [Week 12](week_12/week_12.md) | D — LLM Engineering + RAG + Agents | Retrieval Science & Advanced RAG |
| [Week 13](week_13/week_13.md) | D — LLM Engineering + RAG + Agents | Agent Engineering |
| [Week 14](week_14/week_14.md) | E — Evaluation + Reliability + Security | Evaluation & Reliability Engineering |
| [Week 15](week_15/week_15.md) | E — Evaluation + Reliability + Security | Security & Distributed Systems |
| [Week 16](week_16/week_16.md) | F — RLHF + Agentic RL + Frontier | Post-Training: RLHF, DPO & Alignment |
| [Week 17](week_17/week_17.md) | F — RLHF + Agentic RL + Frontier | Reasoning Models, Agentic RL & Research Thinking |
| [Week 18](week_18/week_18.md) | G — Production Capstone | Capstone: Production AI System + Hiring Sprint |
| [Week 19](week_19/week_19.md) | H — Cloud & AWS for AI | AWS Core: EC2, S3, VPC, IAM, ELB, CloudWatch |
| [Week 20](week_20/week_20.md) | H — Cloud & AWS for AI | AWS for AI: Serverless, Containers & Migration |
| [Week 21](week_21/week_21.md) | I — MLOps, LLMOps & AIOps | MLOps: Pipelines, Tracking, Serving, CI/CD & IaC |
| [Week 22](week_22/week_22.md) | I — MLOps, LLMOps & AIOps | LLMOps & AIOps: Gateways, Observability & Monitoring |
| [Week 23](week_23/week_23.md) | J — Deep Learning & Multimodal | DL Extended: CV, GANs, VAEs & Diffusion |
| [Week 24](week_24/week_24.md) | J — Deep Learning & Multimodal | AI Frameworks, Multimodal & Voice Agents |
| [Week 25](week_25/week_25.md) | K — Responsible & Generative AI | AI Ethics, Responsible AI & Generative Foundations |

---

## The Blocks at a Glance

- **Foundation (Week 0)** — Clean Python, OOP, async, Pydantic, SQL, Git, Linux, Docker. The software base every later week assumes.
- **Block A — Math + ML Foundations (1–4)** — Probability/stats/information theory, linear algebra & optimization, classical ML from scratch, and deep-learning foundations built manually before touching PyTorch.
- **Block B — Transformers + LLM Internals (5–7)** — From RNNs to the full transformer, modern GPT internals (RoPE, GQA, KV cache, Flash Attention), and post-training mechanics (quantization, LoRA/QLoRA, serving).
- **Block C — Systems + Backend + Data (8–10)** — GPU architecture and parallelism, production backend engineering (FastAPI, Postgres, Redis, queues), and the data/vector pipelines that most real AI projects fail on.
- **Block D — LLM Engineering + RAG + Agents (11–13)** — Prompt & context engineering, production-grade retrieval and advanced RAG, and building reliable agents (not toy demos).
- **Block E — Evaluation + Reliability + Security (14–15)** — Eval harnesses, observability, guardrails, deployment strategies, red-teaming, and distributed-systems fundamentals.
- **Block F — RLHF + Agentic RL + Frontier Research (16–17)** — SFT → RLHF → DPO → GRPO, reasoning models, agentic RL, and how to read research critically.
- **Block G — Production Capstone (18)** — Ship one real, deployed system end-to-end across a chosen track (AI SWE, FDE, Applied AI, or AI Infra), plus the hiring sprint.
- **Block H — Cloud & AWS for AI (19–20)** — Core AWS infrastructure and AI-native cloud patterns (serverless, EKS, managed data, migration).
- **Block I — MLOps, LLMOps & AIOps (21–22)** — Reproducible pipelines, experiment tracking, serving, CI/CD, IaC, plus LLM gateways, tracing, and production monitoring.
- **Block J — Deep Learning & Multimodal Depth (23–24)** — CNNs/CV, GANs, VAEs, diffusion, CLIP, and the full framework/memory/voice/multimodal ecosystem.
- **Block K — Responsible & Generative AI (25)** — Generative modeling from first principles, plus bias, hallucination, copyright, and responsible-deployment judgment.

---

## The Production Stack You Will Master

**MLOps:** Python · Git · Linux · Docker · Pandas/NumPy · Spark · Airflow · scikit-learn/PyTorch/TensorFlow/XGBoost · MLflow · W&B · DVC · FastAPI · BentoML · KServe · GitHub Actions/Jenkins/GitLab CI · Terraform · Ansible · Kubernetes · Helm · Prometheus · Grafana · Evidently AI

**LLMOps:** OpenAI/Anthropic/Gemini + Llama/Mistral/Qwen/DeepSeek · LangChain · LlamaIndex · DSPy · Haystack · Pinecone/Weaviate/Milvus/Qdrant/Chroma/FAISS · LangSmith/Phoenix/Ragas/TruLens · vLLM/TGI/Ollama/Ray Serve · Guardrails AI/NeMo Guardrails

**AIOps:** Prometheus · Grafana · ELK/OpenSearch · Jaeger · OpenTelemetry · PagerDuty/Opsgenie · Ansible/Terraform/Argo CD · AWS/Azure/GCP · Docker/Kubernetes

**Highest-ROI path:** Python → Linux → Git → Docker → Kubernetes → AWS → Terraform → MLflow → Airflow → FastAPI → LangChain → LlamaIndex → Vector DB → vLLM → Prometheus → Grafana → ELK → Argo CD

---

## What Good Looks Like at Graduation

**On a whiteboard:** derive attention from scratch · explain KV cache, RoPE, SwiGLU without notes · design a production RAG system · architect a multi-agent system and its failure modes · explain RLHF → DPO → GRPO · roofline analysis for a transformer · CAP theorem applied to a real AI system · design an AWS-deployed AI system end-to-end.

**In code:** ship a full-stack AI product solo · build a RAG pipeline that beats a naive baseline · run and evaluate a LoRA fine-tune · write a working GRPO training loop · deploy and monitor an AI system on AWS with full observability.

**As a portfolio:** weekly LinkedIn/X posts · a deployed capstone with a live demo · at least one merged OSS PR · 3–5 technical write-ups · a clean, documented GitHub.

> This curriculum targets 2026–2030 relevance. Models and frameworks change. The underlying skills — systems thinking, engineering judgment, retrieval science, evaluation discipline, research taste, cloud/ops fluency, and the ability to ship — will not.
