<div align="center">

# 🚀 Enterprise Multi-Environment DevSecOps & GitOps Platform

### Enterprise CI/CD Pipeline using Jenkins Dynamic Kubernetes Agents, GitHub Actions, GitOps, ArgoCD, Kubernetes, Apache Tomcat, DevSecOps, Observability & Service Mesh


![GitHub](https://img.shields.io/badge/GitHub-Source-black?logo=github)
![Jenkins](https://img.shields.io/badge/Jenkins-CI/CD-red?logo=jenkins)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-Automation-blue?logo=githubactions)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Orchestration-blue?logo=kubernetes)
![Docker](https://img.shields.io/badge/Docker-Containers-2496ED?logo=docker)
![Kaniko](https://img.shields.io/badge/Kaniko-Container%20Builder-orange)
![Tomcat](https://img.shields.io/badge/Apache-Tomcat-yellow)
![ArgoCD](https://img.shields.io/badge/ArgoCD-GitOps-EF7B4D?logo=argo)
![Istio](https://img.shields.io/badge/Istio-Service%20Mesh-blue)
![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-E6522C?logo=prometheus)
![Grafana](https://img.shields.io/badge/Grafana-Dashboard-orange?logo=grafana)
![Loki](https://img.shields.io/badge/Loki-Logs-F2C037)
![Tempo](https://img.shields.io/badge/Tempo-Tracing-7B42BC)
![OpenTelemetry](https://img.shields.io/badge/OpenTelemetry-Observability-7B42BC)
![Trivy](https://img.shields.io/badge/Trivy-Security-1904DA)
![Ansible](https://img.shields.io/badge/Ansible-Automation-black?logo=ansible)
![Slack](https://img.shields.io/badge/Slack-Notifications-purple?logo=slack)

---

## Enterprise DevSecOps Platform

A complete production-ready DevSecOps platform demonstrating enterprise software delivery using GitOps, CI/CD, Kubernetes, Apache Tomcat, Jenkins Dynamic Kubernetes Agents, GitHub Actions, Infrastructure Automation, Security Scanning, Monitoring, Logging, Distributed Tracing, and Service Mesh.

The platform supports **multiple deployment environments (Development, Staging and Production)** using parameter-driven pipelines and GitOps.

</div>

---

## Platform Infrastructure

---

# 🏗 Enterprise Platform Infrastructure

This application is built on top of a reusable **Enterprise Platform Engineering** foundation that provides the complete DevSecOps, GitOps, Kubernetes, Observability, and Platform Engineering ecosystem used across all of my projects.

The platform repository includes enterprise-grade infrastructure such as:

- Jenkins Dynamic Kubernetes Agents
- GitHub Actions
- GitOps with ArgoCD
- Kubernetes Cluster Configuration
- Helm Charts
- Docker & Kaniko Image Builds
- Apache Tomcat Deployment
- Nexus Artifact Repository
- Ansible Automation
- Trivy DevSecOps Security Pipeline
- Prometheus Monitoring
- Grafana Dashboards
- Loki Log Aggregation
- Promtail Log Collection
- Tempo Distributed Tracing
- OpenTelemetry
- AlertManager
- Slack Notifications
- Horizontal Pod Autoscaler (HPA)
- Istio Service Mesh
- Multi-Environment Deployments (Development, Staging, Production)
- Enterprise CI/CD Pipelines
- Infrastructure Automation
- Platform Documentation

> **Repository:** Platform-Engineering-Devops-setup *(Private Repository)*

🔒 **Access Notice**

The Platform Engineering repository is currently **private** because it contains reusable enterprise platform components, infrastructure automation, deployment templates, and internal platform configurations shared across multiple projects.

If you are a **recruiter, hiring manager, interviewer, or engineering professional** interested in reviewing the complete Platform Engineering implementation, please contact me to request access.

📧 **Email:** **dasabharat90@gmail.com**

Access requests are reviewed and granted upon request.

---

---

# Architecture


```
Developer
      │
      ▼
 GitHub Repository
      │
      ▼
 GitHub Actions
      │
      ▼
 Jenkins Pipeline
(Dynamic Kubernetes Agents)
      │
      ├─────────────── Build (Maven)
      │
      ├─────────────── Docker / Kaniko
      │
      ├─────────────── Nexus
      │
      ├─────────────── Trivy Scan
      │
      ├─────────────── Ansible Validation
      │
      ├─────────────── GitOps Update
      │
      ▼
 Git Repository
(Environment Manifests)

      │
      ▼

 ArgoCD

      │

      ▼

 Kubernetes Cluster

      │

      ├──── Apache Tomcat

      ├──── HPA

      ├──── Istio

      ├──── Prometheus

      ├──── Grafana

      ├──── Loki

      ├──── Tempo

      ├──── OpenTelemetry

      └──── Slack Alerts
```
<img width="1024" height="1536" alt="ChatGPT Image Jul 4, 2026, 03_09_34 PM" src="https://github.com/user-attachments/assets/d280a834-2c1c-4013-a8e3-7c9fb65e79e9" />

---

# Features

## CI/CD

- Jenkins Dynamic Kubernetes Agents
- GitHub Integration
- GitHub Actions
- Maven Build
- Docker Image Build
- Kaniko Build
- Nexus Artifact Repository
- Parallel Pipeline Execution
- Parameter Driven Deployment
- Multi Environment Support

---

## GitOps

- ArgoCD
- Automatic Sync
- Self Healing
- Git as Single Source of Truth
- Rollback Support
- Environment Specific Deployments

---

## DevSecOps

- Trivy Security Scan
- HTML Security Reports
- Critical Vulnerability Gate
- Infrastructure Validation
- Security First Deployment
- Automated Rollback

---

## Infrastructure Automation

- Kubernetes
- Helm
- Apache Tomcat
- Ansible
- Namespace Automation
- Service Deployment
- Ingress Deployment

---

## Observability

- Prometheus
- Grafana
- AlertManager
- Loki
- Promtail
- Tempo
- OpenTelemetry

---

## Service Mesh

- Istio
- Traffic Management
- Secure Communication
- Future Canary Deployments
- Future Blue Green Deployments

---

<img width="1536" height="1024" alt="ChatGPT Image Jul 4, 2026, 03_02_53 PM" src="https://github.com/user-attachments/assets/1bffbf78-ce8e-4e70-835d-0399d5fb49ef" />


# Repository Structure

```text
devops-multi-env/
│
├── .github/
│   └── workflows/
│
├── ansible/
│   ├── auto-fix.yml
│   ├── precheck.yml
│   └── inventory.ini
│
├── argocd/
│   ├── applications/
│   │   ├── dev.yaml
│   │   ├── staging.yaml
│   │   └── prod.yaml
│   ├── ingress.yaml
│   └── root-app.yaml
│
├── gitops/
│   ├── dev/
│   ├── staging/
│   └── prod/
│
├── helm/
│   └── hello-app/
│
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── servicemonitor.yaml
│   ├── hpa.yaml
│   └── namespace.yaml
│
├── server/
│
├── webapp/
│
├── Dockerfile
├── Jenkinsfile
├── pom.xml
└── README.md
```

---

# Pipeline Workflow

```
GitHub Push

      │

      ▼

GitHub Actions

      │

      ▼

Jenkins Pipeline

      │

Checkout Source

      │

Build Maven

      │

Docker / Kaniko Build

      │

Push Image

      │

Nexus

      │

Ansible Infrastructure Validation

      │

Trivy Security Scan

      │

Security Gate

      │

Publish Reports

      │

GitOps Manifest Update

      │

ArgoCD

      │

Deploy to Kubernetes

      │

Apache Tomcat

      │

HPA

      │

Istio

      │

Prometheus

      │

Grafana

      │

Loki

      │

Tempo

      │

OpenTelemetry

      │

Slack Notifications
```

---

# Deployment Environments

| Environment | Purpose | Namespace |
|------------|----------|-----------|
| Development | Developer Testing | dev |
| Staging | QA Validation | staging |
| Production | Live Traffic | prod |

Pipeline deployment is selected dynamically using Jenkins parameters.

---

# Technology Stack

| Category | Technology |
|-----------|------------|
| Source Control | GitHub |
| CI | GitHub Actions |
| CD | Jenkins |
| Build | Maven |
| Container | Docker |
| Container Builder | Kaniko |
| Registry | Docker Hub / Nexus |
| GitOps | ArgoCD |
| Container Platform | Kubernetes |
| Application Server | Apache Tomcat |
| Security | Trivy |
| Automation | Ansible |
| Monitoring | Prometheus |
| Dashboards | Grafana |
| Logging | Loki |
| Log Collector | Promtail |
| Tracing | Tempo |
| Telemetry | OpenTelemetry |
| Service Mesh | Istio |
| Alerts | AlertManager |
| Notifications | Slack |

---

# Security

✔ Automated Vulnerability Scanning

✔ Infrastructure Validation

✔ Critical Vulnerability Blocking

✔ Security Reports

✔ Automated Rollback

✔ GitOps Based Deployment

---

# Observability

- Metrics → Prometheus
- Dashboards → Grafana
- Logs → Loki + Promtail
- Traces → Tempo
- Telemetry → OpenTelemetry
- Alerts → AlertManager
- Notifications → Slack

---

# Future Enhancements

- Blue Green Deployment
- Canary Deployment
- Progressive Delivery
- KEDA Event Driven Autoscaling
- Policy as Code (OPA/Gatekeeper)
- Secrets Management (Vault)
- AI-powered Self-Healing
- Chaos Engineering
- Cost Optimization Dashboard

---

# Author

**Bharat Dasa**

**Enterprise DevOps | DevSecOps | Kubernetes | GitOps | Cloud | Platform Engineering | Observability**

---

## ⭐ If you found this project useful, consider giving it a Star.
