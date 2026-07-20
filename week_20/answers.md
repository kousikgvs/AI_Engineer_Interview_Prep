# Week 20 — Answers with Code: AWS for AI — Serverless, Containers & Migration

> Serverless patterns, containers, and migration with boto3/config snippets.

---

## Serverless & Event-Driven

### 1. Lambda cold starts (detailed)
First invocation (or scale-up) initializes a new env: download code, start runtime, run init. Big packages/VPC/heavy imports worsen it; provisioned concurrency avoids it.

### 2. Lambda vs EKS for serving (detailed)
Lambda: lightweight, spiky, short (embeddings, webhooks), zero ops, no GPU. EKS: GPU inference, long-running/large models, full control. Choose by size/latency/traffic.

### 3. API Gateway throttling (easy)
Rate/burst throttling, usage plans (per-key quotas), API keys, auth → protect backends and control cost.

### 4. SQS visibility timeout & DLQ (detailed)
Read hides a message (visibility timeout) so others skip it; if not deleted in time it reappears; after N failures it moves to a DLQ.

### 5. SNS vs SES (easy)
SNS: pub/sub fan-out to queues/Lambdas/HTTP. SES: sending email.

### 6. Serverless embedding pipeline (detailed + code)
S3 upload → event → Lambda → embed → vector DB.
```python
def handler(event, context):
    for rec in event["Records"]:
        key = rec["s3"]["object"]["key"]
        text = read_s3(rec["s3"]["bucket"]["name"], key)
        for chunk in chunk_text(text):
            vec = embed(chunk)
            vector_db.upsert(id=hash(chunk), values=vec, metadata={"key": key})
```

---

## Containers & Orchestration

### 7. Containerize a model service (code)
```dockerfile
FROM python:3.11-slim
COPY requirements.txt . 
RUN pip install --no-cache-dir -r requirements.txt
COPY app/ ./app
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]
```

### 8. ECS vs EKS vs Fargate (easy)
ECS (AWS-native), EKS (managed k8s, portable), Fargate (serverless containers, no nodes).

### 9. Kubernetes basics (detailed)
Pods (containers), Deployments (replicas/rollout), Services (stable networking), HPA (autoscale on metrics). GPU nodes + node selectors for inference.

### 10. Rolling vs blue-green vs canary (detailed)
Rolling: replace gradually. Blue-green: switch traffic between two full envs. Canary: small % first, then ramp. Trade rollback speed vs cost.

---

## Migration & Cost

### 11. Migration strategies (6 R's) (easy)
Rehost (lift-shift), Replatform, Refactor, Repurchase, Retire, Retain — choose per workload effort/value.

### 12. Cost optimization (detailed)
Right-size, Spot for batch, autoscale to zero, caching, S3 lifecycle tiers, reserved for steady load, budgets + Cost Explorer.

### 13. Multi-region / DR (easy)
Replicate data + deploy across regions; failover via DNS (Route 53); trade cost vs RTO/RPO.

---

## Coding

### 14. SQS consumer with retry/DLQ (code)
```python
def consume(msg):
    try:
        process(msg["Body"]); sqs.delete_message(QueueUrl=q, ReceiptHandle=msg["ReceiptHandle"])
    except Exception:
        pass  # not deleted -> reappears after visibility timeout -> DLQ after N tries
```

### 15. Lambda + provisioned concurrency: config to avoid cold starts on hot paths.

---

## Judgment / Failure

### 16. Lambda times out on big model: wrong compute → move to EKS/ECS with GPU or SageMaker endpoint.
### 17. Messages lost/duplicated: no DLQ/idempotency → DLQ + idempotent consumers + right visibility timeout.
### 18. Migration stalls: big-bang refactor → incremental strangler pattern, migrate hot paths first.
