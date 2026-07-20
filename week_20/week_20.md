# Week 20 — AWS for AI: Serverless, Containers & Migration

**Block:** H — Cloud & AWS for AI (Advanced Depth Track)
**Goal:** Move from raw infrastructure to AI-native cloud patterns — serverless inference, container orchestration, managed data services, and migrating real workloads to AWS.

---

## What You Will Learn

### Serverless & Event-Driven AWS
- AWS Lambda — functions, triggers, cold starts, memory/timeout tuning
- Lambda for AI: lightweight inference, embeddings, webhook handlers
- API Gateway — REST and HTTP APIs, throttling, API keys, usage plans
- API Gateway + Lambda — the serverless AI API pattern
- SQS — queues, visibility timeout, dead letter queues
- SNS & SES — pub/sub notifications and transactional email
- Event-driven architecture on AWS (S3 events → Lambda → SQS)

### Databases on AWS
- RDS — managed Postgres/MySQL, read replicas, Multi-AZ
- DynamoDB — partition/sort keys, single-table design, on-demand vs provisioned
- When to use RDS vs DynamoDB for AI workloads (session state, metadata, vectors)
- ElastiCache (Redis) — managed caching and rate limiting

### Containers & Orchestration
- ECR — container registry, pushing and pulling images
- EKS — managed Kubernetes: clusters, node groups, pods, deployments
- Kubernetes basics: pods, services, deployments, autoscaling (HPA)
- Helm — packaging Kubernetes apps
- Model serving on Kubernetes — vLLM / TGI on GPU node groups
- Autoscaling model servers based on request load

### Managed AI & Data Services (Concept Level)
- SageMaker overview — training, endpoints, pipelines
- Bedrock — hosted foundation models on AWS
- Kinesis — real-time data streaming (vs Kafka tradeoffs)

### On-Premises → AWS Migration
- Migration strategies: rehost, replatform, refactor (the 6 R's)
- Assessment and planning — dependency mapping
- Data migration approaches
- Migrating a monolith AI service to cloud-native AWS
- Cost optimization post-migration (spot instances, reserved capacity, right-sizing)

---

## Project
- Deploy a serverless AI API: API Gateway → Lambda → LLM call, with SQS for async jobs and a DLQ
- Containerize a vLLM model server, push to ECR, deploy on EKS with GPU node group and horizontal autoscaling
- Back it with RDS Postgres (metadata) + DynamoDB (session state) + ElastiCache (rate limiting)
- Write a migration plan: take a localhost AI service and produce an ADR + architecture for moving it to AWS

---

## Paper / Reading
- Read: "Designing Data-Intensive Applications" — Chapter 5 (Replication) + revisit Kleppmann on partitioning
- Reference: AWS Serverless Application Lens (Well-Architected)

---

## Failure Friday
- Analyze: A serverless AI API that blew its budget overnight — Lambda recursion / no concurrency limits / no SQS backpressure. How to cap and alert.

---

## Interview Questions This Week Prepares You For
- "When would you use Lambda vs a container on EKS for model serving?"
- "Design a serverless pipeline that processes uploaded documents into embeddings"
- "RDS or DynamoDB for storing agent session state? Defend it"
- "How would you migrate a monolithic AI app to AWS with zero downtime?"

---

## Engineering Judgment Question
**"Serverless (Lambda) or containers (EKS) for this model-serving workload?"**
Write your answer covering: what you would do, why, what tradeoff you are making, and what alternative you rejected.
