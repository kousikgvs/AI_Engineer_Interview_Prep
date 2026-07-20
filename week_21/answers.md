# Week 21 — Answers with Code: MLOps — Pipelines, Tracking, Serving, CI/CD & IaC

> Lifecycle, drift, experiment tracking, serving, and IaC with code.

---

## Lifecycle & Drift

### 1. MLOps lifecycle (detailed)
Data ingestion/validation → features → training + tracking → eval → deploy/serve → monitor (drift/perf) → retrain when metrics degrade — automated, versioned loop.

### 2. Data vs concept vs model drift (detailed)
Data drift: input distribution shifts. Concept drift: input→output relationship changes. Model drift: performance degrades. Detect via input stats + monitored metrics.

### 3. Notebook ≠ production (easy)
Notebooks lack reproducibility, versioning, testing, monitoring, scaling, automated deploy. Production needs pipelines + CI/CD + observability.

### 4. Detect drift (code)
```python
from scipy.stats import ks_2samp
def data_drift(ref, live, alpha=0.05):
    stat, p = ks_2samp(ref, live)      # distribution shift test
    return p < alpha                    # True = drift detected
```

---

## Orchestration

### 5. Pandas → Spark (easy)
Move to Spark when data exceeds RAM or needs distributed parallelism; Pandas fine in-memory.

### 6. Airflow DAG (detailed + code)
```python
from airflow import DAG
from airflow.operators.python import PythonOperator
with DAG("train", schedule="@daily") as dag:
    ingest = PythonOperator(task_id="ingest", python_callable=ingest_fn)
    train = PythonOperator(task_id="train", python_callable=train_fn)
    ingest >> train   # dependency
```

### 7. Idempotent pipeline tasks (easy)
Deterministic, re-runnable tasks (upserts, partition overwrite) so retries/backfills don't corrupt data.

---

## Experiment Tracking & Registry

### 8. Experiment tracking (code)
```python
import mlflow
with mlflow.start_run():
    mlflow.log_params({"lr": lr, "batch": bs})
    mlflow.log_metric("val_acc", acc)
    mlflow.log_artifact("model.pkl")
```

### 9. Model registry (easy)
Versioned models with stages (staging/production), lineage, and approvals → controlled promotion + rollback.

### 10. Data/model versioning (easy)
DVC/lakeFS for data, registry for models → reproduce any run from versioned inputs.

---

## Serving

### 11. Online vs batch inference (easy)
Online: low-latency per-request endpoint. Batch: score large datasets offline (cheaper, higher throughput).

### 12. Model serving endpoint (code)
```python
from fastapi import FastAPI
import joblib
app = FastAPI(); model = joblib.load("model.pkl")
@app.post("/predict")
def predict(features: list[float]):
    return {"pred": model.predict([features])[0].tolist()}
```

### 13. Shadow / canary deployment (detailed)
Shadow: send live traffic to new model without serving its output (compare). Canary: serve to small % then ramp. Safe validation on real data.

---

## CI/CD & IaC

### 14. CI/CD for ML (detailed)
Test data/code → train → eval gate (block if metrics drop) → register → deploy → monitor. Automate on commit.

### 15. IaC / Terraform (code)
```hcl
resource "aws_s3_bucket" "artifacts" {
  bucket = "ml-artifacts"
}
resource "aws_sagemaker_endpoint" "model" {
  endpoint_config_name = aws_sagemaker_endpoint_configuration.cfg.name
}
```
Version infra as code → reproducible, reviewable environments.

### 16. Feature store (easy)
Central, versioned features shared across training/serving → consistency, no train/serve skew.

---

## Coding

### 17. Drift detection (see Q4).
### 18. MLflow tracking (see Q8).
### 19. Serving endpoint (see Q12).

---

## Judgment / Failure

### 20. Model silently degrades in prod: no monitoring → drift + performance metrics, alerts, auto-retrain trigger.
### 21. Train/serve skew: different feature code paths → feature store, shared transforms.
### 22. Can't reproduce a result: no versioning → version data + code + config + model in the registry.
