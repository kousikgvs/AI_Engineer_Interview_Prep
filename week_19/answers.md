# Week 19 — Answers with Code: AWS Core for AI Engineers

> AWS fundamentals — mostly conceptual with CLI/boto3 snippets.

---

## Fundamentals & IAM

### 1. Shared responsibility model (detailed)
AWS secures *of* the cloud (hardware, network, managed infra); you secure *in* the cloud (data, IAM, config, encryption). Split shifts with how managed the service is.

### 2. Region / AZ / edge (easy)
Region = geographic area; AZ = isolated data center (HA); edge = CDN cache near users.

### 3. IAM least privilege (detailed + code)
Deny by default; grant narrow actions/resources.
```json
{ "Effect": "Allow", "Action": ["s3:GetObject"],
  "Resource": "arn:aws:s3:::my-bucket/prefix/*" }
```

### 4. EC2 → S3 without hardcoded creds (detailed)
Attach an IAM role to the instance; SDK auto-uses temporary rotating creds from instance metadata — no keys in code.

### 5. Access keys vs roles (detailed)
Long-lived keys leak and don't rotate; roles give short-lived, auto-rotated, scoped creds → prefer roles.

---

## Compute

### 6. EC2 families/AMIs (easy)
Choose by workload (compute/memory/GPU-optimized); size for load; AMI = launch image. g/p families for model serving/training.

### 7. Auto Scaling (easy)
Scale instances on metrics (CPU/queue depth) between min/max → match capacity to load, save cost.

### 8. Spot vs On-Demand vs Reserved (detailed)
Spot: cheap, interruptible (batch/training with checkpoints). On-Demand: flexible, pricey. Reserved/Savings: commit for discount (steady load).

### 9. Lambda / serverless (easy)
Event-driven, auto-scaling, pay-per-invoke; good for glue/light inference, limited by timeout/memory/cold starts.

### 10. Containers: ECS vs EKS vs Fargate (easy)
ECS (AWS-native orchestration), EKS (managed Kubernetes), Fargate (serverless containers, no node management).

---

## Storage & Data

### 11. S3 storage classes (easy)
Standard → Infrequent Access → Glacier → Deep Archive: cheaper storage, higher retrieval cost/latency.

### 12. S3 for ML data (code)
```python
import boto3
s3 = boto3.client("s3")
s3.upload_file("model.bin", "my-bucket", "models/v1/model.bin")
s3.download_file("my-bucket", "data/train.parquet", "train.parquet")
```

### 13. EBS vs EFS vs S3 (easy)
EBS: block volume for one instance. EFS: shared NFS across instances. S3: object storage for data lakes/artifacts.

### 14. VPC basics (detailed)
Isolated network; public subnets (internet-facing) + private subnets (backend/DB); security groups (stateful) + NACLs (stateless) control traffic.

---

## AI/ML Services

### 15. Bedrock (easy)
Managed API for foundation models (Claude, Titan, etc.) with no infra; supports RAG (Knowledge Bases) and agents.

### 16. SageMaker (easy)
End-to-end ML: training jobs, hosted endpoints, pipelines; for custom models beyond Bedrock.

### 17. Invoke Bedrock (code)
```python
import boto3, json
br = boto3.client("bedrock-runtime")
resp = br.invoke_model(
    modelId="anthropic.claude-3-sonnet-20240229-v1:0",
    body=json.dumps({"messages": [{"role": "user", "content": "Hi"}],
                     "max_tokens": 256, "anthropic_version": "bedrock-2023-05-31"}))
print(json.loads(resp["body"].read()))
```

---

## Coding / Ops

### 18. Presigned URL (code)
```python
url = s3.generate_presigned_url("get_object",
    Params={"Bucket": "b", "Key": "k"}, ExpiresIn=3600)  # temp scoped access
```

### 19. CloudWatch monitoring (easy)
Metrics, logs, alarms; alarm on latency/error/cost → auto-scale or notify.

---

## Judgment / Failure

### 20. Leaked AWS keys in code: hardcoded creds → IAM roles, Secrets Manager, rotate immediately, scan repos.
### 21. Surprise GPU bill: idle On-Demand GPUs → Spot for batch, auto-scale to zero, right-size instances, budgets/alerts.
### 22. Over-permissive IAM: wildcard policies → least privilege, scoped resources, Access Analyzer.
