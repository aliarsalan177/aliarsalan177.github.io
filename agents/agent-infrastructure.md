# Agent Guide — Infrastructure — Cloud to Code

> The path from a git push to production: containers, CI/CD, Infrastructure as Code, environments, and observability.
> Category: Architecture & Systems · Source: https://aliarsalan177.github.io/guides/infrastructure/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Infrastructure — Cloud to Code. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Infrastructure as Code
- ✅ **APPLY:** Define infra declaratively (Terraform/Pulumi/CloudFormation) in version control; review and apply via pipelines.
- ⛔ **AVOID:** Click-ops in the cloud console — undocumented, unrepeatable, and impossible to review or roll back.

### 2. Containerize for parity
- ✅ **APPLY:** Package apps in Docker (small, multi-stage images); orchestrate with Kubernetes/ECS for scaling and self-healing.
- ⛔ **AVOID:** 'Works on my machine' snowflake servers and fat images with secrets baked in.

### 3. Automate CI/CD
- ✅ **APPLY:** On push: lint, test, build, scan, then deploy through environments (dev → staging → prod) with rollbacks.
- ⛔ **AVOID:** Manual, ad-hoc deploys with no tests or gates — the classic source of production incidents.

### 4. Config & secrets by environment
- ✅ **APPLY:** Externalize config via env vars; store secrets in a manager (Vault, AWS Secrets Manager, SSM), not in code.
- ⛔ **AVOID:** Hard-coded credentials, committing .env, and per-environment code branches.

### 5. Observe and secure everything
- ✅ **APPLY:** Ship logs, metrics, traces and alerts; least-privilege IAM, network segmentation, TLS, and automated backups.
- ⛔ **AVOID:** Deploying blind with no monitoring, over-broad IAM roles, and untested backups.

## Cheat Reference — concepts to remember

- **Infrastructure as Code** — Define infra declaratively (Terraform/Pulumi/CloudFormation) in version control; review and apply via pipelines.
- **Containerize for parity** — Package apps in Docker (small, multi-stage images); orchestrate with Kubernetes/ECS for scaling and self-healing.
- **Automate CI/CD** — On push: lint, test, build, scan, then deploy through environments (dev → staging → prod) with rollbacks.
- **Config & secrets by environment** — Externalize config via env vars; store secrets in a manager (Vault, AWS Secrets Manager, SSM), not in code.
- **Observe and secure everything** — Ship logs, metrics, traces and alerts; least-privilege IAM, network segmentation, TLS, and automated backups.

## Full Cheat Sheet — every concept

### Cloud Foundations
- Providers: AWS/GCP/Azure; compute (EC2/VMs, containers, serverless/Lambda), storage (S3/blob), managed DBs.
- Networking: VPC, subnets, security groups, load balancers, DNS, CDN.
- IAM: least-privilege roles/policies; regions & availability zones for HA.

### Containers & Orchestration
- Docker: multi-stage builds, small base images, .dockerignore, no secrets in layers.
- Kubernetes/ECS: deployments, services, ingress, autoscaling, health probes, self-healing.
- Registries; image scanning; resource limits/requests.

### IaC & Config
- Terraform/Pulumi/CloudFormation in git; plan → review → apply via pipeline; remote state.
- 12-factor config via env vars; secrets in Vault/Secrets Manager/SSM.
- Immutable infrastructure; environment parity (dev/staging/prod).

### CI/CD
- Pipeline: install → lint → test → build → scan → deploy → verify.
- GitHub Actions/GitLab CI/Jenkins; artifact/image build; environment gates & approvals.
- Deploy strategies: rolling, blue-green, canary; automated rollback.

### Observability & Reliability
- Logs (ELK/Loki), metrics (Prometheus/Grafana/CloudWatch), traces (OpenTelemetry), alerting.
- SLIs/SLOs/SLAs; on-call/runbooks; incident response.
- Backups + tested restores, DR plan, TLS everywhere, WAF/DDoS protection.

## Interview Questions

#### Q1. What is Infrastructure as Code and why use it?
Defining infrastructure (servers, networks, databases) in declarative, version-controlled files (Terraform, CloudFormation) so environments are repeatable, reviewable, and auditable — instead of manual console changes.

#### Q2. Walk through a CI/CD pipeline.
On a push/PR: install deps, lint, run tests, build artifacts (e.g. a Docker image), scan for vulnerabilities, then deploy to staging and (after approval/gates) production — with automated rollback on failure.

#### Q3. Containers vs virtual machines?
VMs virtualize hardware with a full guest OS (heavier, stronger isolation); containers share the host kernel and package just the app + deps (lightweight, fast, portable). Containers give environment parity and density; orchestrators manage them at scale.

#### Q4. How do you manage secrets and config across environments?
Externalize config as environment variables per environment, and keep secrets in a dedicated manager (Vault, AWS Secrets Manager/SSM, sealed secrets) injected at runtime — never committed to the repo or baked into images.

#### Q5. Blue-green vs canary deployments?
Blue-green keeps two environments and switches traffic all at once (instant rollback). Canary shifts a small percentage of traffic to the new version, watches metrics, then ramps up — limiting blast radius of a bad release.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
