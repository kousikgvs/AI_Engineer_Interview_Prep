# Week 19 — Interview Questions: AWS Core for AI Engineers

> Click a question (▸) to expand the answer.

---

## Fundamentals & IAM

<details>
<summary>1. What is the shared responsibility model?</summary>

AWS secures the cloud (hardware, network, managed-service infrastructure); you secure what's in the cloud (data, IAM, OS/app config, encryption). The split shifts with how managed the service is.
</details>

<details>
<summary>2. Regions vs availability zones vs edge locations?</summary>

A region is a geographic area; it contains multiple isolated availability zones (data centers) for high availability; edge locations are CDN/caching points close to users.
</details>

<details>
<summary>3. How does IAM enforce least privilege?</summary>

Policies grant only the specific actions/resources needed, attached to users/groups/roles. Deny by default; grant narrowly; use conditions — so a compromised principal can do minimal damage.
</details>

<details>
<summary>4. Give an EC2 instance S3 access without hardcoded creds?</summary>

Attach an IAM role to the instance; the SDK automatically uses temporary rotating credentials from the instance metadata — no keys in code.
</details>

<details>
<summary>5. Access keys vs IAM roles — why roles safer?</summary>

Long-lived access keys can leak and don't rotate. Roles provide short-lived, auto-rotated temporary credentials scoped to a task — no secrets to manage or leak.
</details>

## Compute — EC2 & Auto Scaling

<details>
<summary>6. EC2 instance families/AMIs — how to choose?</summary>

Pick a family by workload (compute-, memory-, GPU-optimized), size for the load, and use an AMI (preconfigured image) as the launch template. GPU instances (g/p families) for model serving/training.
</details>

<details>
<summary>7. Security group vs network ACL?</summary>

Security groups are stateful, instance-level, allow-only rules. Network ACLs are stateless, subnet-level, and support allow and deny. SGs are the primary control; NACLs add coarse subnet-level filtering.
</details>

<details>
<summary>8. How do Auto Scaling Groups work?</summary>

An ASG maintains a desired number of instances across AZs, replaces unhealthy ones, and scales in/out on metrics (CPU, requests) via scaling policies — matching capacity to demand.
</details>

<details>
<summary>9. Start/stop/terminate + status via boto3?</summary>

Use the EC2 client: `run_instances`, `start_instances`, `stop_instances`, `terminate_instances`, and `describe_instance_status` — automating lifecycle management in Python.
</details>

## Storage

<details>
<summary>10. S3 storage classes, versioning, lifecycle?</summary>

Classes trade cost vs access speed (Standard, Infrequent Access, Glacier). Versioning keeps object history. Lifecycle policies auto-transition/expire objects to save cost.
</details>

<details>
<summary>11. EBS vs EFS vs S3 for AI workloads?</summary>

EBS: block storage for one instance (fast, for training scratch/DBs). EFS: shared file system across instances. S3: object storage for datasets, model artifacts, and embeddings.
</details>

<details>
<summary>12. What is an EBS snapshot; recovery?</summary>

A point-in-time backup of a volume stored in S3; you restore by creating a new volume from the snapshot — for backups and cloning.
</details>

## Networking

<details>
<summary>13. Walk through a VPC.</summary>

A VPC is your private network. Public subnets route to an internet gateway; private subnets reach the internet via a NAT gateway. Route tables direct traffic; security groups/NACLs filter it.
</details>

<details>
<summary>14. What happens when a request hits your ALB?</summary>

The ALB terminates SSL, evaluates listener/routing rules, health-checks targets, and forwards the request to a healthy instance in a target group, distributing load.
</details>

<details>
<summary>15. ALB vs NLB?</summary>

ALB is layer-7 (HTTP routing, path/host rules, good for web/APIs). NLB is layer-4 (TCP, ultra-low latency, high throughput, static IPs).
</details>

<details>
<summary>16. Route 53 and ACM?</summary>

Route 53 is managed DNS with routing policies (latency, weighted, failover). ACM provisions and auto-renews SSL/TLS certificates for HTTPS.
</details>

<details>
<summary>17. What is VPC peering?</summary>

A private network connection between two VPCs so resources communicate using private IPs without going over the public internet.
</details>

## Security & Observability

<details>
<summary>18. What is KMS; when encrypt?</summary>

Key Management Service manages encryption keys. Use it to encrypt data at rest (S3, EBS, RDS) and in transit for sensitive data and compliance.
</details>

<details>
<summary>19. Secrets Manager vs SSM Parameter Store?</summary>

Both store config/secrets securely; Secrets Manager adds automatic rotation and is purpose-built for secrets, while Parameter Store is cheaper for general config.
</details>

<details>
<summary>20. CloudWatch vs CloudTrail?</summary>

CloudWatch = metrics, logs, alarms, dashboards (operational monitoring). CloudTrail = audit log of every API call (who did what, when) for security/compliance.
</details>

## System Design

<details>
<summary>21. Provision a production VPC + EC2 behind an ALB with an ASG.</summary>

VPC with public + private subnets across AZs, internet + NAT gateways, an ALB in public subnets routing to an ASG of instances in private subnets, security groups least-privileged, secrets in Secrets Manager, and CloudWatch/CloudTrail enabled.
</details>

## Failure / Judgment

<details>
<summary>22. Public S3 bucket leaked PII — fix it.</summary>

Root cause: public bucket policy, no encryption, no auditing. Fix: block public access, least-privilege bucket policy, KMS encryption, enable CloudTrail + access logging, and scan for other exposed buckets.
</details>

<details>
<summary>23. "IAM role or access keys for this service-to-service call?"</summary>

**What I'd do:** IAM role with temporary credentials.
**Why:** no long-lived secrets to leak; auto-rotated and scoped.
**Tradeoff:** roles need correct trust configuration.
**Rejected:** static access keys — they leak and rarely get rotated.
</details>
