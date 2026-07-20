# Week 20 — Interview Questions: AWS for AI — Serverless, Containers & Migration

> Sourced from cloud/infra + FDE screens. Focus on AI-native cloud patterns.

---

## Serverless & Event-Driven
1. AWS Lambda: cold starts, memory/timeout tuning — what causes cold starts?
2. **When would you use Lambda vs a container on EKS for model serving?**
3. API Gateway: throttling, usage plans, API keys.
4. SQS visibility timeout and dead-letter queues — how do they work?
5. SNS vs SES — pub/sub vs transactional email.
6. **Design a serverless pipeline: S3 upload → Lambda → embeddings → vector DB.**

## Databases on AWS
7. RDS Multi-AZ vs read replicas — availability vs scaling.
8. DynamoDB: partition/sort keys, single-table design, on-demand vs provisioned.
9. **RDS or DynamoDB for agent session state? Defend it.**
10. What does ElastiCache (Redis) give you?

## Containers & Orchestration
11. What is ECR? How do you push/pull images?
12. EKS basics: clusters, node groups, pods, deployments.
13. What is a Horizontal Pod Autoscaler? How do you autoscale model servers?
14. What is Helm?
15. How do you serve vLLM/TGI on GPU node groups?

## Managed AI Services
16. SageMaker vs Bedrock — when do you use each?
17. Kinesis vs Kafka — tradeoffs.

## Migration
18. What are the 6 R's of migration (rehost, replatform, refactor, ...)?
19. **How would you migrate a monolithic AI app to AWS with minimal downtime?**
20. Post-migration cost optimization: spot vs reserved vs right-sizing.

## Failure / Judgment
21. A serverless AI API blew its budget overnight (Lambda recursion, no concurrency limits) — fix it.
22. "Serverless (Lambda) or containers (EKS) for this model-serving workload?" — defend it.
