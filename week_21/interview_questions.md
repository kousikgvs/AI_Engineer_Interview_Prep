# Week 21 — Interview Questions: MLOps — Pipelines, Tracking, Serving, CI/CD & IaC

> Click a question (▸) to expand the answer.

---

## Lifecycle & Drift

<details>
<summary>1. Walk me through the MLOps lifecycle from data to retraining.</summary>

Data ingestion/validation → feature engineering → training + experiment tracking → evaluation → deployment (serving) → monitoring (drift, performance) → trigger retraining when metrics degrade — an automated, versioned loop.
</details>

<details>
<summary>2. Model drift vs data drift vs concept drift?</summary>

Data drift: input distribution changes. Concept drift: the input→output relationship changes. Model drift: performance degrades over time. Detect via input distribution stats and monitored prediction/label metrics.
</details>

<details>
<summary>3. Why is "it works in my notebook" not production?</summary>

Notebooks lack reproducibility, versioning, testing, monitoring, scaling, and automated deployment. Production needs pipelines, CI/CD, and observability so results are reproducible and reliable.
</details>

## Data Processing & Orchestration

<details>
<summary>4. When do you outgrow Pandas and reach for Spark?</summary>

When data exceeds single-machine memory or needs distributed processing/parallelism. Spark scales across a cluster; Pandas is fine for data that fits in RAM.
</details>

<details>
<summary>5. Airflow DAGs, operators, sensors, backfills?</summary>

A DAG defines task dependencies; operators run work; sensors wait for conditions; the scheduler runs on a cadence; backfills re-run historical intervals. It orchestrates reproducible pipelines.
</details>

## Experiment Tracking & Versioning

<details>
<summary>6. MLflow or Weights & Biases — how to choose?</summary>

Both track params/metrics/artifacts. MLflow is open-source, self-hostable, with a model registry (good for control/on-prem). W&B has richer UI/collaboration/sweeps (great for research teams). Choose by hosting, collaboration, and budget.
</details>

<details>
<summary>7. What goes in a model registry?</summary>

Versioned models with metadata: metrics, training data/code versions, stage (staging/production), and lineage — enabling controlled promotion and rollback.
</details>

<details>
<summary>8. Make an experiment reproducible six months later?</summary>

Pin and version data (DVC), code (Git), config/hyperparameters, environment (Docker/requirements), and random seeds; log everything to a tracker so any run can be recreated exactly.
</details>

<details>
<summary>9. What does DVC add on top of Git?</summary>

Git handles code but not large datasets/models. DVC versions large files (stored in S3/GCS) with lightweight pointers in Git, linking data versions to code commits.
</details>

## Serving

<details>
<summary>10. FastAPI vs BentoML vs KServe vs Ray Serve?</summary>

FastAPI: general Python API (DIY). BentoML: packaging/serving ML models with batching. KServe: Kubernetes-native serverless model serving with autoscaling. Ray Serve: scalable Python serving with composition. Choose by scale and infra.
</details>

<details>
<summary>11. Batch vs online vs streaming inference?</summary>

Batch: scheduled bulk predictions (offline). Online: real-time per-request (low latency). Streaming: continuous on event streams. Pick by latency needs and data arrival pattern.
</details>

## CI/CD & IaC

<details>
<summary>12. Design a CI/CD pipeline that safely deploys a new model version.</summary>

On merge: run data/model/unit tests, train/validate, compare against the current model on a golden set, register if better, deploy via canary/shadow, monitor, and auto-roll-back on regression.
</details>

<details>
<summary>13. What automated tests for ML?</summary>

Data tests (schema, ranges, nulls), model tests (min accuracy, no regression on slices), behavioral/invariance tests, and integration tests of the serving path.
</details>

<details>
<summary>14. Continuous training; Argo CD / GitOps?</summary>

Continuous training automatically retrains on new data/drift. GitOps (Argo CD) makes Git the source of truth — the cluster syncs to declared manifests, giving auditable, reproducible deploys.
</details>

<details>
<summary>15. Terraform vs Ansible; why IaC?</summary>

Terraform provisions infrastructure declaratively (create resources). Ansible configures/manages existing servers (procedural). IaC makes environments reproducible, version-controlled, and reviewable instead of manual clicks.
</details>

## System Design

<details>
<summary>16. Design an end-to-end MLOps pipeline.</summary>

Airflow ingests+validates → DVC versions data → training logged to MLflow and registered → model served via BentoML/FastAPI in Docker → GitHub Actions CI/CD deploys on merge → Terraform provisions infra → Evidently monitors drift and triggers retraining.
</details>

## Failure / Judgment

<details>
<summary>17. Model degraded from data drift — which stage should catch it?</summary>

The monitoring stage should have detected input drift / performance drop and triggered retraining. Root cause: no drift monitoring or retrain trigger. Add distribution monitoring, alerts, and automated retraining.
</details>

<details>
<summary>18. "MLflow or W&B for this team?"</summary>

**What I'd do:** MLflow for open-source/self-hosted control with a model registry; W&B for rich collaboration/sweeps and managed convenience.
**Why:** matches hosting and collaboration needs.
**Tradeoff:** MLflow needs more setup; W&B is a paid managed dependency.
**Rejected:** the other when hosting/budget/collaboration constraints don't fit.
</details>
