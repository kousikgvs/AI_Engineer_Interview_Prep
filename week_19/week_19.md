# Week 19 — AWS Core for AI Engineers

**Block:** H — Cloud & AWS for AI (Advanced Depth Track)
**Goal:** Every production AI system runs on cloud infrastructure. Master the AWS core that FDE and infrastructure roles assume you already know.

> Why this week exists: Forward Deployed Engineer and AI Infrastructure roles (Palantir, Together AI, startups) expect real cloud fluency. The core sprint deploys to Railway/Render; this track takes you to production AWS.

---

## What You Will Learn

### AWS Overview & Account Foundations
- What the cloud is; regions, availability zones, edge locations
- The shared responsibility model
- AWS Management Console, CLI, and SDKs (boto3)
- Billing basics and the AWS Pricing Calculator

### Identity & Access Management (IAM)
- Users, groups, roles, and policies
- Principle of least privilege
- IAM roles for services (EC2 instance roles, service roles)
- Policy documents — JSON structure, allow/deny, conditions
- Access keys vs roles — why roles are safer

### Compute — EC2 & Auto Scaling
- EC2 instances — types, families, AMIs, key pairs
- Security groups — inbound/outbound rules (attach, detach)
- Launching, starting, stopping, terminating instances
- Checking instance status
- Auto Scaling Groups (ASG) — scaling policies, health checks
- Managing EC2 with Python (boto3): create, launch, start/stop/terminate

### Storage — S3, EBS, EFS
- S3 — buckets, objects, storage classes, versioning, lifecycle policies
- S3 for AI: datasets, model artifacts, embeddings, static assets
- EBS — block storage volumes, attaching to EC2
- EBS Snapshots — backups and recovery
- EFS — shared elastic file system across instances

### Networking — VPC & Route 53
- VPC fundamentals (Part 1): subnets (public/private), route tables
- VPC (Part 2): internet gateway, NAT gateway
- VPC (Part 3): network ACLs vs security groups
- VPC Peering — connecting VPCs
- Elastic Load Balancer (ELB) — ALB vs NLB, target groups
- Route 53 — DNS, hosted zones, routing policies
- ACM — SSL/TLS certificate management

### Security & Secrets
- KMS — key management and encryption
- Systems Manager (SSM) — parameter store, session manager
- Secrets Manager — storing DB credentials and API keys

### Observability & Governance
- CloudWatch — metrics, logs, alarms, dashboards
- CloudTrail — audit logging of API activity

---

## Project
- Provision a VPC with public and private subnets, an internet gateway, and a NAT gateway
- Launch an EC2 instance behind an ALB with an Auto Scaling Group
- Store model artifacts in S3 with versioning and lifecycle policies
- Manage secrets in Secrets Manager; encrypt an EBS volume with KMS
- Automate EC2 lifecycle (create, start, stop, terminate) with a boto3 Python script
- Wire CloudWatch alarms and CloudTrail auditing

---

## Paper / Reading
- Read: AWS Well-Architected Framework — the five pillars (operational excellence, security, reliability, performance efficiency, cost optimization)

---

## Failure Friday
- Analyze: A public S3 bucket that leaked training data / customer PII — misconfigured bucket policy, no encryption, no CloudTrail. How least privilege and guardrails prevent it.

---

## Interview Questions This Week Prepares You For

<details>
<summary>"Explain the difference between a security group and a network ACL"</summary>

Security groups are stateful, instance-level, allow-only rules. Network ACLs are stateless, subnet-level, and support both allow and deny. SGs are the primary control; NACLs add coarse subnet filtering.
</details>

<details>
<summary>"How would you give an EC2 instance access to S3 without hardcoding credentials?"</summary>

Attach an IAM role to the instance; the SDK automatically uses temporary, auto-rotated credentials from instance metadata — no keys in code.
</details>

<details>
<summary>"Walk me through what happens when a request hits your ALB"</summary>

The ALB terminates SSL, evaluates listener/routing rules, health-checks targets, and forwards the request to a healthy instance in a target group, balancing load.
</details>

<details>
<summary>"How does IAM enforce least privilege?"</summary>

Policies grant only the specific actions/resources needed, attached to users/groups/roles; deny by default and grant narrowly with conditions — so a compromised principal can do minimal damage.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"IAM role or access keys for this service-to-service call?"</strong></summary>

**What I'd do:** IAM role with temporary credentials.
**Why:** no long-lived secrets to leak; auto-rotated and scoped.
**Tradeoff:** roles need correct trust configuration.
**Rejected:** static access keys — they leak and rarely get rotated.
</details>
