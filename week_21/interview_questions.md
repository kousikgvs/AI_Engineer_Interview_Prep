# Week 21 — Interview Questions: MLOps — Pipelines, Tracking, Serving, CI/CD & IaC

> Sourced from MLOps / ML-platform engineer screens.

---

## Lifecycle & Drift
1. **Walk me through the MLOps lifecycle from data to retraining.**
2. Model drift vs data drift vs concept drift — how do you detect each?
3. Why is "it works in my notebook" not production?

## Data Processing & Orchestration
4. When do you outgrow Pandas and reach for Spark?
5. Airflow DAGs, operators, sensors, backfills — how do you build a reproducible pipeline?

## Experiment Tracking & Versioning
6. **MLflow or Weights & Biases — how do you choose?**
7. What goes in a model registry?
8. **How do you make an ML experiment reproducible six months later?** (DVC + pinned env + code)
9. What does DVC add on top of Git?

## Serving
10. FastAPI vs BentoML vs KServe vs Ray Serve — when to use each?
11. Batch vs online vs streaming inference.

## CI/CD & IaC
12. **Design a CI/CD pipeline that safely deploys a new model version.**
13. What automated tests do you run for ML (data tests, model tests, regression tests)?
14. What is continuous training? What is Argo CD / GitOps?
15. Terraform vs Ansible — what does each do? Why does IaC matter for reproducibility?

## System Design
16. Design an end-to-end MLOps pipeline: ingest → version → train → register → serve → monitor → retrain.

## Failure / Judgment
17. A model silently degraded from data drift — no monitoring, no retrain trigger. Which stage should have caught it?
18. "MLflow or W&B for this team?" — defend it.
