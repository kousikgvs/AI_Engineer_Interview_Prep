# Week 21 — MLOps: Pipelines, Tracking, Serving, CI/CD & IaC

**Block:** I — MLOps, LLMOps & AIOps (Advanced Depth Track)
**Goal:** Turn one-off models and notebooks into reproducible, versioned, automatically deployed systems. This is the industry-ready stack that gets ML engineers hired in 2026.

---

## What You Will Learn

### The MLOps Lifecycle
- MLOps lifecycle: data → train → validate → deploy → monitor → retrain
- Roles and responsibilities across the lifecycle
- Model drift, data drift, and concept drift — detection and response
- Why "it works in my notebook" is not production

### Data Processing & Orchestration
- Pandas & NumPy at scale; when you outgrow them
- Apache Spark — distributed data processing
- Apache Airflow — DAGs, operators, scheduling, sensors, backfills
- Building reproducible data pipelines

### Experiment Tracking
- MLflow — experiments, runs, params, metrics, artifacts, model registry
- Weights & Biases — tracking, sweeps, dashboards
- Comparing runs and reproducing results

### Data & Model Versioning
- DVC — data version control alongside Git
- Model versioning and lineage
- Reproducibility: pinning data + code + environment

### Model Development & Training
- Scikit-learn, XGBoost, PyTorch, TensorFlow in a pipeline
- Structuring training code: Dataset, DataLoader, Model, Trainer classes
- Hyperparameter search and tracking

### Model Serving
- FastAPI for model APIs
- BentoML — packaging and serving models
- KServe — Kubernetes-native model serving
- Ray Serve — scalable Python serving
- Batch vs online vs streaming inference

### CI/CD for ML
- GitHub Actions, Jenkins, GitLab CI/CD
- Automated testing for ML (data tests, model tests, regression tests)
- Continuous training and continuous deployment pipelines
- Argo CD — GitOps deployment

### Infrastructure as Code (IaC)
- Terraform — provisioning cloud infra declaratively
- Ansible — configuration management
- Why IaC matters for reproducible ML environments

---

## Project
- Build an end-to-end MLOps pipeline:
  1. Airflow DAG ingests and validates data
  2. DVC versions the dataset
  3. Training run logged to MLflow (params, metrics, model artifact) with the model registered
  4. Model served via FastAPI/BentoML, packaged in Docker
  5. GitHub Actions CI/CD builds, tests, and deploys on merge
  6. Terraform provisions the underlying infra
- Add drift detection with Evidently AI and trigger a retrain

---

## Paper / Reading
- Read: "Hidden Technical Debt in Machine Learning Systems" — Sculley et al. (Google)

---

## Failure Friday
- Analyze: A model that silently degraded in production because of data drift — no monitoring, no retraining trigger. What pipeline stage should have caught it?

---

## Interview Questions This Week Prepares You For

<details>
<summary>"Walk me through your MLOps lifecycle from data to retraining"</summary>

Data ingestion/validation → feature engineering → training + experiment tracking → evaluation → deployment → monitoring (drift/performance) → trigger retraining when metrics degrade — an automated, versioned loop.
</details>

<details>
<summary>"How do you make an ML experiment reproducible six months later?"</summary>

Pin and version data (DVC), code (Git), config/hyperparameters, environment (Docker), and seeds; log everything to a tracker so any run can be recreated exactly.
</details>

<details>
<summary>"What is model drift and how do you detect it?"</summary>

Performance degrading over time from data/concept drift. Detect by monitoring input distributions and prediction/label metrics and alerting when they shift beyond thresholds.
</details>

<details>
<summary>"Design a CI/CD pipeline that safely deploys a new model version"</summary>

On merge: run data/model/unit tests, train/validate, compare against production on a golden set, register if better, deploy via canary/shadow, monitor, and auto-roll-back on regression.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"MLflow or Weights & Biases for this team's experiment tracking?"</strong></summary>

**What I'd do:** MLflow for open-source/self-hosted control with a model registry; W&B for rich collaboration/sweeps and managed convenience.
**Why:** matches hosting and collaboration needs.
**Tradeoff:** MLflow needs more setup; W&B is a paid managed dependency.
**Rejected:** the other when hosting/budget/collaboration constraints don't fit.
</details>
