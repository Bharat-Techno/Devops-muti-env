# 🚀 Jenkins DevSecOps Pipeline Documentation

> Complete documentation of the Enterprise Multi-Environment CI/CD Pipeline.

---

# Overview

This project uses an enterprise-grade Jenkins Declarative Pipeline running on **dynamic Kubernetes agents**. The pipeline automates the complete software delivery lifecycle, including:

- Source Code Checkout
- Application Build
- Container Image Creation
- Security Scanning
- Infrastructure Validation
- GitOps Deployment
- Kubernetes Deployment
- Monitoring Verification
- Health Validation
- Automated Rollback

The pipeline follows DevSecOps best practices by integrating security, automation, GitOps, observability, and infrastructure validation directly into the deployment process.

---

# Pipeline Architecture

> Replace with Pipeline Hero Image

```
Developer

        │

        ▼

GitHub Repository

        │

        ▼

GitHub Actions (Optional Trigger)

        │

        ▼

Jenkins

Dynamic Kubernetes Agent

        │

        ▼

Checkout

        │

        ▼

Build (Maven)

        │

        ▼

Docker / Kaniko Build

        │

        ▼

Docker Hub / Nexus

        │

        ▼

Ansible Auto Fix

        │

        ▼

Trivy Security Scan

        │

        ▼

Pre Deployment Validation

        │

        ▼

Publish HTML Report

        │

        ▼

Deploy Kubernetes

        │

        ▼

Deploy HPA

        │

        ▼

Deploy Monitoring

        │

        ▼

Verify Prometheus

        │

        ▼

Health Check

        │

        ▼

Rollback (If Required)

        │

        ▼

Slack Notification
```

---

# Pipeline Technologies

| Category | Tool |
|------------|----------------|
| SCM | GitHub |
| CI | Jenkins |
| Agents | Kubernetes Dynamic Agents |
| Build | Maven |
| Container | Docker |
| Alternative Builder | Kaniko |
| Registry | Docker Hub / Nexus |
| Security | Trivy |
| Infrastructure | Ansible |
| Deployment | Kubernetes |
| GitOps | ArgoCD |
| Application Server | Apache Tomcat |
| Autoscaling | HPA |
| Monitoring | Prometheus |
| Dashboards | Grafana |
| Logging | Loki |
| Tracing | Tempo |
| Telemetry | OpenTelemetry |
| Notifications | Slack |

---

# Dynamic Kubernetes Agent

The Jenkins controller never performs builds directly.

Instead, Jenkins provisions a temporary Kubernetes Pod for every pipeline execution.

Example Pod Containers

```
jnlp
maven
docker
kubectl
ansible
trivy
kaniko
```

Advantages

- Clean environment every build
- Better scalability
- No dependency conflicts
- Parallel execution
- Faster cleanup

---

# Environment Variables

Pipeline Variables

| Variable | Description |
|------------|------------------------------|
| IMAGE_NAME | Docker image name |
| TAG | Build Number |
| NAMESPACE | Kubernetes Namespace |
| PROM_URL | Prometheus API URL |
| ANSIBLE_HOST_KEY_CHECKING | Disable SSH host verification |

Example

```groovy
environment {
    IMAGE_NAME="bharatdasa/hello-world"
    TAG="${BUILD_NUMBER}"
}
```

---

# Stage 1 — Checkout

Purpose

Clone application source code from GitHub.

Technology

- Git
- GitHub

Pipeline

```
GitHub

↓

Checkout

↓

Workspace
```

Jenkins

```groovy
git credentialsId:'github-ssh'
```

Output

Complete application source code.

---

# Stage 2 — Build

Purpose

Compile Java application and create WAR package.

Technology

- Maven

Command

```bash
mvn clean package -DskipTests
```

Produces

```
target/hello-app.war
```

---

# Stage 3 — Docker Build & Push

Purpose

Package application into a container image.

Technology

- Docker

Steps

```
Build Image

↓

Tag Image

↓

Login Docker Hub

↓

Push Image
```

Output

```
docker.io/bharatdasa/hello-world:BUILD_NUMBER
```

Future Enhancement

Replace Docker daemon with Kaniko for daemonless image builds inside Kubernetes.

---

# Stage 4 — Ansible Auto-Fix Infrastructure

Purpose

Automatically validate and repair infrastructure before deployment.

Technology

- Ansible

Playbook

```
auto-fix.yml
```

Tasks

- Verify Servers
- Validate Services
- Restart Failed Services
- Fix Configuration
- Infrastructure Readiness

Benefits

Prevents deployment failures caused by unhealthy infrastructure.

---

# Stage 5 — Parallel Security Checks

Purpose

Run multiple security checks simultaneously to reduce pipeline execution time.

---

## 5.1 HTML Report

Technology

Trivy

Produces

```
reports/trivy-report.html
```

Report Contains

- Vulnerabilities
- CVEs
- Severity
- Installed Packages

Published inside Jenkins.

---

## 5.2 Critical Security Gate

Purpose

Block deployment when critical vulnerabilities exist.

Command

```bash
trivy image \
--severity CRITICAL \
--exit-code 1
```

Result

```
No Critical Issues

↓

Continue Deployment
```

OR

```
Critical Issues Found

↓

Pipeline Failed
```

---

# Stage 6 — Pre Deployment Validation

Technology

Ansible

Playbook

```
precheck.yml
```

Checks

- Server Reachability
- Disk Space
- Memory
- Services
- Ports
- Kubernetes Readiness

---

# Stage 7 — Publish Trivy Report

Purpose

Publish security report to Jenkins.

Plugin

HTML Publisher Plugin

Report

```
Trivy Security Report
```

Recruiters love seeing published security reports because it demonstrates DevSecOps maturity.

---

# Stage 8 — Deploy

Technology

- kubectl
- Kubernetes

Steps

```
Namespace

↓

Deployment

↓

Service

↓

Ingress

↓

Rollout Status
```

Files Applied

```
namespace.yaml

deployment.yaml

service.yaml

ingress.yaml
```

Image Tag Updated

```
IMAGE_PLACEHOLDER

↓

bharatdasa/hello-world:BUILD_NUMBER
```

---

# Stage 9 — Deploy HPA

Technology

Horizontal Pod Autoscaler

File

```
hpa.yaml
```

Purpose

Automatically scale pods according to CPU or Memory usage.

Flow

```
Traffic

↓

Metrics

↓

HPA

↓

Scale Pods
```

---

# Stage 10 — Deploy Monitoring

Technology

Prometheus Operator

File

```
servicemonitor.yaml
```

Purpose

Expose application metrics for Prometheus scraping.

Flow

```
Application

↓

ServiceMonitor

↓

Prometheus

↓

Grafana
```

---

# Stage 11 — Verify Monitoring

Purpose

Validate Prometheus connectivity before completing deployment.

API

```
http://prometheus/api/v1/query?query=up
```

Success

```
Prometheus Reachable
```

Failure

```
Pipeline Failed
```

---

# Stage 12 — Health Check & Rollback

Purpose

Verify deployed application health.

Checks

- Failed Pods
- CrashLoopBackOff
- HPA Status
- Deployment Status

If Failure

```
kubectl rollout undo
```

Benefits

Automatic recovery without manual intervention.

---

# Post Actions

## Always

Archive

```
Trivy HTML Reports
```

---

## Success

Display

```
Deployment Successful
```

---

## Failure

Display

```
Deployment Failed
```

Pipeline exits immediately.

---

# Complete Pipeline Flow

```
Developer

↓

GitHub

↓

GitHub Actions

↓

Jenkins

↓

Checkout

↓

Build

↓

Docker Build

↓

Docker Push

↓

Ansible Auto Fix

↓

Parallel Trivy Scan

↓

Critical Security Gate

↓

Infrastructure Validation

↓

Publish HTML Report

↓

Deploy Kubernetes

↓

Deploy HPA

↓

Deploy Monitoring

↓

Verify Monitoring

↓

Health Check

↓

Rollback (If Required)

↓

Slack Notification
```

---

# Security Gates

The pipeline includes built-in quality gates.

| Validation | Result |
|-------------|---------|
| Maven Build | Must Pass |
| Docker Build | Must Pass |
| Docker Push | Must Pass |
| Infrastructure Validation | Must Pass |
| Trivy Critical Scan | Must Pass |
| Kubernetes Rollout | Must Pass |
| Prometheus Validation | Must Pass |
| Pod Health | Must Pass |

If any validation fails, the pipeline stops immediately to prevent unstable or insecure deployments.

---

# Multi-Environment Deployment

The pipeline is designed to support parameter-driven deployments.

Supported environments:

```
Development

↓

Staging

↓

Production
```

Each environment can have:

- Dedicated Namespace
- Dedicated Helm Values
- Dedicated ArgoCD Application
- Dedicated GitOps Manifests
- Independent Configuration

---

# DevSecOps Best Practices

✔ Infrastructure as Code

✔ Immutable Container Images

✔ GitOps Workflow

✔ Automated Security Scanning

✔ Parallel Pipeline Execution

✔ Infrastructure Validation

✔ Automatic Rollback

✔ Continuous Monitoring

✔ Observability Integration

✔ Kubernetes Native Deployment

---

# Future Improvements

- SonarQube Code Quality Gates
- OWASP Dependency Check
- SBOM Generation (CycloneDX)
- Cosign Image Signing
- Sigstore Verification
- HashiCorp Vault Secrets
- Blue-Green Deployment
- Canary Deployment with Istio
- Progressive Delivery using Argo Rollouts
- KEDA Event-Driven Autoscaling
- AI-Powered Self-Healing
- Chaos Engineering with LitmusChaos

---

# Pipeline Summary

This Jenkins pipeline demonstrates a modern enterprise DevSecOps workflow that integrates CI, security, infrastructure validation, GitOps, Kubernetes deployment, observability, and automated rollback into a single automated process.

It provides a scalable, secure, and production-ready software delivery platform capable of supporting multiple deployment environments while following cloud-native best practices.