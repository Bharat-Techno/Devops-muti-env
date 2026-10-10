# 🔄 GitOps Deployment with ArgoCD

> Enterprise GitOps workflow using GitHub, Jenkins, ArgoCD, Kubernetes, Helm, Apache Tomcat, and Multi-Environment deployments.

---

# Overview

This project follows the **GitOps** methodology for Kubernetes deployments.

Instead of manually applying Kubernetes manifests or directly modifying cluster resources, the desired state of the cluster is stored in Git.

ArgoCD continuously monitors the Git repository and automatically synchronizes Kubernetes clusters whenever changes are detected.

Git becomes the **Single Source of Truth**.

---

# What is GitOps?

GitOps is an operational framework that applies DevOps best practices to Kubernetes by using Git repositories as the authoritative source for infrastructure and application configuration.

Benefits include:

- Declarative deployments
- Version-controlled infrastructure
- Automatic synchronization
- Self-healing
- Easy rollback
- Full audit trail
- Reduced manual intervention

---

# GitOps Architecture

> Replace this section with the GitOps Hero Image

```text
                    Developer

                        │

                  Git Push / PR

                        │

                        ▼

                 GitHub Repository

                        │

                        ▼

                 GitHub Actions

                        │

                        ▼

              Jenkins CI Pipeline

                        │

      Build • Test • Package • Scan

                        │

                        ▼

            Docker / Kaniko Build

                        │

                        ▼

              Docker Hub / Nexus

                        │

                        ▼

      Update GitOps Repository

     deployment.yaml

     values.yaml

     image tag

                        │

                 Git Commit

                        │

                  Git Push

                        │

                        ▼

                     ArgoCD

            Watches Git Repository

                        │

                  Detect Change

                        │

                        ▼

             Synchronize Cluster

                        │

                        ▼

                  Kubernetes

                        │

                        ▼

                Apache Tomcat Pods

                        │

                        ▼

               Monitoring Stack
```

---

# Why GitOps?

Traditional deployments

```
Developer

↓

Jenkins

↓

kubectl apply

↓

Kubernetes
```

Problems

- Manual deployments
- No audit trail
- Drift
- Difficult rollback
- Cluster changes outside Git

---

GitOps deployments

```
Developer

↓

GitHub

↓

Jenkins

↓

GitOps Repository

↓

ArgoCD

↓

Kubernetes
```

Advantages

- Git controls everything
- Automatic synchronization
- Rollback using Git
- Self-healing
- Auditable

---

# Repository Structure

```text
devops-multi-env/

├── argocd/
│
│   ├── applications/
│   │      ├── dev.yaml
│   │      ├── staging.yaml
│   │      └── prod.yaml
│   │
│   └── root-app.yaml
│
├── gitops/
│
│   ├── dev/
│   │      deployment.yaml
│   │      service.yaml
│   │      ingress.yaml
│   │
│   ├── staging/
│   │
│   └── prod/
│
├── helm/
│
└── Jenkinsfile
```

---

# GitOps Components

## GitHub

Stores

- Kubernetes manifests
- Helm charts
- Environment configurations
- ArgoCD Applications

---

## Jenkins

Responsibilities

- Build application
- Run tests
- Create Docker image
- Push image
- Run Trivy scan
- Execute Ansible validation
- Update GitOps manifests
- Commit changes

---

## Docker Registry

Stores immutable container images.

Example

```
hello-world:101

hello-world:102

hello-world:103
```

---

## GitOps Repository

Stores

```
deployment.yaml

service.yaml

ingress.yaml

values.yaml
```

Desired state only.

---

## ArgoCD

Continuously watches Git.

Whenever Git changes

```
Git Change

↓

Detect

↓

Sync

↓

Deploy
```

---

## Kubernetes

Receives deployments only from ArgoCD.

No manual changes.

---

# Deployment Flow

```text
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

Maven Build

↓

Docker Build

↓

Docker Push

↓

Trivy Scan

↓

Ansible Validation

↓

Update deployment.yaml

↓

Git Commit

↓

Git Push

↓

ArgoCD Detects Change

↓

Sync

↓

Deploy

↓

Running Application
```

---

# Image Update Flow

Current image

```yaml
image:

  repository: bharatdasa/hello-world

  tag: "100"
```

Pipeline updates

```yaml
tag: "101"
```

Jenkins performs

```
sed

↓

Update Image Tag

↓

Commit

↓

Push
```

ArgoCD notices the change automatically.

---

# Multi-Environment GitOps

Repository

```text
gitops/

├── dev/

├── staging/

└── prod/
```

Each environment has

- Deployment
- Service
- Ingress
- ConfigMap
- Secrets
- HPA
- ServiceMonitor

---

# ArgoCD Applications

```
applications/

dev.yaml

staging.yaml

prod.yaml
```

Each watches

```
gitops/dev
```

```
gitops/staging
```

```
gitops/prod
```

---

# Root Application

The project uses the **App of Apps** pattern.

```
root-app

↓

Development

↓

Staging

↓

Production
```

Benefits

- Easy onboarding
- Central management
- Scalable

---

# Synchronization

ArgoCD checks Git continuously.

```
Git Repository

↓

Compare

↓

Cluster

↓

Difference?

↓

YES

↓

Sync
```

---

# Self Healing

Suppose someone manually changes a Deployment.

```
kubectl edit deployment
```

ArgoCD detects

```
Cluster != Git
```

Automatically

```
Sync

↓

Restore Desired State
```

---

# Drift Detection

Git

```
replicas: 3
```

Cluster

```
replicas: 7
```

Result

```
OutOfSync
```

ArgoCD fixes automatically.

---

# Rollback

If version 105 fails

```
Git History

↓

Version 104

↓

Git Revert

↓

Push

↓

ArgoCD Sync

↓

Rollback
```

No manual kubectl commands required.

---

# Helm Integration

Instead of raw YAML

```
helm/

hello-app/
```

Environment values

```
values-dev.yaml

values-staging.yaml

values-prod.yaml
```

Advantages

- Less duplication
- Easier maintenance
- Cleaner configuration

---

# Sync Policies

Automatic

```yaml
syncPolicy:

  automated:

    prune: true

    selfHeal: true
```

Capabilities

- Auto Sync
- Auto Heal
- Auto Prune

---

# GitOps Security

Deployment only happens after

✔ Maven Build

✔ Docker Build

✔ Trivy Scan

✔ Critical Security Gate

✔ Ansible Validation

✔ Successful Image Push

Only validated images reach GitOps.

---

# GitOps + Jenkins

Current pipeline

```text
Jenkins

↓

kubectl apply

↓

Cluster
```

Recommended production pipeline

```text
Jenkins

↓

Build

↓

Scan

↓

Docker Push

↓

Update GitOps

↓

Commit

↓

Push

↓

ArgoCD

↓

Deploy
```

This completely removes deployment logic from Jenkins.

---

# Advantages

- Git is the single source of truth
- Full deployment history
- Immutable releases
- Easy rollback
- Self-healing
- Drift detection
- Reduced manual operations
- Better security
- Better auditing
- Declarative deployments

---

# GitOps Workflow Example

Developer pushes code.

```
Git Push
```

↓

Jenkins builds

```
Image

↓

hello-world:104
```

↓

Jenkins updates

```yaml
image:

  tag: "104"
```

↓

Commit

```
Update image to 104
```

↓

Push

↓

ArgoCD detects

↓

Sync

↓

Deployment

↓

Pods restart

↓

Traffic routed

↓

Monitoring starts

---

# Observability Integration

Every GitOps deployment is monitored.

Metrics

- Prometheus

Dashboards

- Grafana

Logs

- Loki

Traces

- Tempo

Telemetry

- OpenTelemetry

Alerts

- AlertManager

Notifications

- Slack

---

# Best Practices

- Never modify Kubernetes resources manually.
- Keep every deployment declarative.
- Use immutable image tags.
- Separate manifests by environment.
- Enable automatic synchronization.
- Enable self-healing and pruning.
- Protect the GitOps repository with branch protection and pull requests.
- Review manifest changes before merging.
- Monitor ArgoCD sync status and application health.

---

# Future Enhancements

- Argo Rollouts
- Progressive Delivery
- Canary Releases
- Blue-Green Deployments
- Multi-Cluster GitOps
- ApplicationSets
- Image Updater
- Vault Integration
- OPA Gatekeeper
- Kyverno Policies
- Disaster Recovery Clusters

---

# GitOps Summary

This project implements an enterprise-ready GitOps workflow where Git acts as the authoritative source for Kubernetes deployments. Jenkins is responsible for building, testing, scanning, and publishing container images, while ArgoCD continuously reconciles the desired state stored in Git with the actual state of the Kubernetes cluster.

By separating **Continuous Integration (CI)** from **Continuous Delivery (CD)**, the platform achieves secure, repeatable, auditable, and self-healing deployments across Development, Staging, and Production environments.