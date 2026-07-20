# Week 20 — Interview Questions: AWS for AI — Serverless, Containers & Migration

> Click a question (▸) to expand the answer.

---

## Serverless & Event-Driven

<details>
<summary>1. Lambda cold starts — what causes them?</summary>

The first invocation (or after scale-up) must initialize a new execution environment — download code, start the runtime, run init. Big packages, VPC attachment, and heavy imports worsen it; provisioned concurrency avoids it.
</details>

<details>
<summary>2. Lambda vs a container on EKS for model serving?</summary>

Lambda for lightweight, spiky, short tasks (embeddings, webhooks) with zero ops but limits on memory/time/GPU. EKS for GPU inference, long-running/large models, and full control. Choose by model size, latency, and traffic pattern.
</details>

<details>
<summary>3. API Gateway throttling, usage plans, API keys?</summary>

API Gateway fronts your APIs with rate/burst throttling, usage plans (per-key quotas), API keys for identification, and auth — protecting backends and controlling cost.
</details>

<details>
<summary>4. SQS visibility timeout and dead-letter queues?</summary>

When a consumer reads a message it's hidden (visibility timeout) so others don't process it; if not deleted in time it reappears. After N failed attempts it moves to a DLQ for inspection.
</details>

<details>
<summary>5. SNS vs SES?</summary>

SNS is pub/sub for fan-out notifications to many subscribers (queues, Lambdas, HTTP). SES is for sending transactional/marketing email.
</details>

<details>
<summary>6. Design a serverless pipeline: S3 → Lambda → embeddings → vector DB.</summary>

S3 upload event triggers Lambda (or drops to SQS for buffering), a worker Lambda chunks and embeds the doc, writes vectors to the vector DB with metadata, and a DLQ + alarms handle failures. Cap concurrency to control cost.
</details>

## Databases on AWS

<details>
<summary>7. RDS Multi-AZ vs read replicas?</summary>

Multi-AZ = synchronous standby in another AZ for failover/availability (not for scaling reads). Read replicas = asynchronous copies that scale read traffic.
</details>

<details>
<summary>8. DynamoDB: partition/sort keys, single-table design?</summary>

The partition key distributes data; the sort key orders items within a partition. Single-table design stores multiple entity types in one table using key patterns/GSIs for access patterns — fast at scale but requires upfront access-pattern design.
</details>

<details>
<summary>9. RDS or DynamoDB for agent session state?</summary>

DynamoDB for high-scale, simple key-value session lookups with low latency and auto-scaling; RDS if sessions are relational and you need complex queries/transactions. Sessions are usually key-value → DynamoDB.
</details>

<details>
<summary>10. What does ElastiCache (Redis) give you?</summary>

Managed in-memory caching for sub-ms reads, rate limiting, and session storage — offloading the database and cutting latency.
</details>

## Containers & Orchestration

<details>
<summary>11. What is ECR; push/pull images?</summary>

Elastic Container Registry stores Docker images. You authenticate, `docker push` to it, and EKS/ECS pull images from it to run containers.
</details>

<details>
<summary>12. EKS basics: clusters, node groups, pods, deployments?</summary>

EKS is managed Kubernetes. A cluster has node groups (EC2/Fargate); pods run containers; deployments manage replica sets for rolling updates and self-healing.
</details>

<details>
<summary>13. What is an HPA; autoscale model servers?</summary>

The Horizontal Pod Autoscaler scales pod count on metrics (CPU, GPU, custom request/queue depth) so model servers match load automatically.
</details>

<details>
<summary>14. What is Helm?</summary>

A Kubernetes package manager — templated, versioned "charts" that deploy and configure apps repeatably instead of hand-writing YAML.
</details>

<details>
<summary>15. Serve vLLM/TGI on GPU node groups?</summary>

Package the server in a container, deploy on a GPU node group with resource requests, front it with a service/load balancer, and autoscale on request/queue metrics.
</details>

## Managed AI Services

<details>
<summary>16. SageMaker vs Bedrock?</summary>

SageMaker: build/train/deploy your own models with full control. Bedrock: call hosted foundation models via API without managing infrastructure. Use Bedrock for speed, SageMaker for custom models.
</details>

<details>
<summary>17. Kinesis vs Kafka?</summary>

Kinesis is AWS-managed streaming (less ops, tight AWS integration). Kafka is more flexible/portable and higher-throughput but you operate it (or use MSK). Choose by ops appetite and ecosystem.
</details>

## Migration

<details>
<summary>18. What are the 6 R's of migration?</summary>

Rehost (lift-and-shift), Replatform (lift-tinker-shift), Repurchase (move to SaaS), Refactor (re-architect cloud-native), Retire (decommission), Retain (keep on-prem). They rank effort vs cloud benefit.
</details>

<details>
<summary>19. Migrate a monolithic AI app to AWS with minimal downtime?</summary>

Assess dependencies, containerize, migrate data (replicate then cut over), run old and new in parallel, shift traffic gradually (canary/DNS weighting), verify, then decommission — with rollback ready.
</details>

<details>
<summary>20. Post-migration cost optimization?</summary>

Right-size instances, use spot for fault-tolerant/batch work, reserved/savings plans for steady load, autoscaling, S3 lifecycle policies, and cost dashboards/alerts.
</details>

## Failure / Judgment

<details>
<summary>21. Serverless AI API blew its budget overnight — fix it.</summary>

Root cause: Lambda recursion or unbounded concurrency and no backpressure. Fix with reserved concurrency limits, SQS buffering, idempotency, cost alarms/budgets, and circuit breakers.
</details>

<details>
<summary>22. "Serverless (Lambda) or containers (EKS) for this model-serving workload?"</summary>

**What I'd do:** Lambda for light, spiky, short CPU tasks; EKS for GPU/large/long-running model serving.
**Why:** Lambda is zero-ops and scales to zero; EKS gives GPUs and control.
**Tradeoff:** Lambda's memory/time/no-GPU limits vs EKS's ops burden.
**Rejected:** Lambda for heavy GPU inference; EKS for tiny sporadic tasks.
</details>
