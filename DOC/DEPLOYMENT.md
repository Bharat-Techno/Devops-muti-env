# 🚀 Enterprise Platform Deployment Guide

> Complete deployment guide for the Enterprise Multi-Environment DevSecOps & GitOps Platform.

---

# Table of Contents

- Overview
- Architecture
- Prerequisites
- Infrastructure Requirements
- Repository Setup
- Jenkins Setup
- Kubernetes Setup
- Docker Registry Setup
- Nexus Setup
- GitHub Actions Setup
- Ansible Setup
- ArgoCD Setup
- Monitoring Setup
- Istio Setup
- Application Deployment
- Multi-Environment Deployment
- Verification
- Rollback
- Troubleshooting

---

# Overview

This guide explains how to deploy the entire Enterprise DevSecOps Platform from scratch.

The platform consists of:

- GitHub
- GitHub Actions
- Jenkins
- Dynamic Kubernetes Agents
- Docker
- Kaniko
- Nexus
- ArgoCD
- Kubernetes
- Apache Tomcat
- Ansible
- Trivy
- Prometheus
- Grafana
- Loki
- Promtail
- Tempo
- OpenTelemetry
- AlertManager
- Slack

---

# High-Level Deployment Flow

Developer

↓

GitHub

↓

GitHub Actions

↓

Jenkins

↓

Docker / Kaniko

↓

Docker Hub / Nexus

↓

GitOps

↓

ArgoCD

↓

Kubernetes

↓

Apache Tomcat

↓

Monitoring

↓

Slack Alerts

---

# Prerequisites

Minimum Requirements

| Component | Version |
|------------|----------|
| Ubuntu | 24.04 |
| Kubernetes | 1.30+ |
| Docker | Latest |
| Helm | Latest |
| kubectl | Latest |
| Jenkins | LTS |
| Java | 17 |
| Maven | 3.9+ |
| Git | Latest |
| Ansible | Latest |

Hardware

Minimum

8 CPU

16 GB RAM

100 GB SSD

Recommended

16 CPU

32 GB RAM

250 GB SSD

---

# Clone Repository

```bash
git clone git@github.com:<username>/devops-multi-env.git

cd devops-multi-env
```

---

# Repository Structure

```text
devops-multi-env/

.github/

argocd/

gitops/

helm/

k8s/

ansible/

server/

webapp/

Jenkinsfile

Dockerfile

pom.xml
```

---

# Configure GitHub

Create Repository

↓

Push Code

↓

Generate SSH Key

↓

Add Deploy Key

↓

Configure Branch Protection

↓

Enable GitHub Actions

---

# Configure Jenkins

Install Plugins

- Kubernetes
- Git
- Docker
- Pipeline
- HTML Publisher
- Credentials
- SSH Agent
- Blue Ocean

Create Credentials

GitHub SSH

DockerHub

Ansible SSH

Slack Token

Create Kubernetes Cloud

↓

Configure Pod Template

Containers

- jnlp
- maven
- docker
- kubectl
- trivy
- ansible
- kaniko

---

# Configure Kubernetes

Create Namespace

```bash
kubectl apply -f k8s/namespace.yaml
```

Verify

```bash
kubectl get ns
```

---

# Configure Docker Registry

Login

```bash
docker login
```

Test Push

```bash
docker push username/test
```

---

# Configure Nexus

Deploy Nexus

↓

Create Docker Hosted Repository

↓

Create Maven Repository

↓

Create Credentials

↓

Configure Jenkins

---

# Configure GitHub Actions

Repository

↓

Settings

↓

Secrets

↓

Add

GITHUB_TOKEN

DOCKER_USERNAME

DOCKER_PASSWORD

JENKINS_URL

JENKINS_TOKEN

---

# Configure Ansible

Inventory

```ini
[kubernetes]

master

worker1

worker2
```

Test

```bash
ansible all -m ping
```

---

# Configure ArgoCD

Install

```bash
kubectl create namespace argocd

helm install argocd argo/argo-cd
```

Deploy Root Application

```bash
kubectl apply -f argocd/root-app.yaml
```

Verify

```bash
kubectl get applications -n argocd
```

---

# Configure Prometheus

```bash
helm install monitoring prometheus-community/kube-prometheus-stack
```

Verify

```bash
kubectl get pods -n monitoring
```

---

# Configure Loki

```bash
helm install loki grafana/loki
```

---

# Configure Promtail

```bash
helm install promtail grafana/promtail
```

---

# Configure Tempo

```bash
helm install tempo grafana/tempo
```

---

# Configure OpenTelemetry

```bash
helm install otel open-telemetry/opentelemetry-collector
```

---

# Configure Istio

Install

```bash
istioctl install
```

Enable Injection

```bash
kubectl label namespace project1 istio-injection=enabled
```

Verify

```bash
kubectl get pods
```

---

# Configure AlertManager

Apply

```bash
kubectl apply -f monitoring/alertmanager.yaml
```

Configure Slack Webhook

↓

Restart AlertManager

---

# Configure Slack

Create Incoming Webhook

↓

Copy Webhook URL

↓

Store in Jenkins Credentials

↓

Configure AlertManager

---

# Build Application

```bash
mvn clean package
```

Produces

```
target/hello-app.war
```

---

# Build Docker Image

```bash
docker build -t hello-world:v1 .
```

Push

```bash
docker push hello-world:v1
```

---

# Deploy with Jenkins

Open Jenkins

↓

Create Pipeline

↓

Point to Repository

↓

Run Build

Stages

- Checkout
- Build
- Docker
- Trivy
- Ansible
- Deploy
- Monitoring
- Verification

---

# Manual Kubernetes Deployment

```bash
kubectl apply -f k8s/
```

Verify

```bash
kubectl get all -n project1
```

---

# GitOps Deployment

Update

deployment.yaml

↓

Commit

↓

Push

↓

ArgoCD Sync

↓

Deploy

---

# Multi-Environment Deployment

Development

```bash
ENV=dev
```

Staging

```bash
ENV=staging
```

Production

```bash
ENV=prod
```

Pipeline selects environment automatically.

---

# Verify Deployment

Pods

```bash
kubectl get pods
```

Services

```bash
kubectl get svc
```

Ingress

```bash
kubectl get ingress
```

HPA

```bash
kubectl get hpa
```

---

# Verify Monitoring

Prometheus

```bash
kubectl get pods -n monitoring
```

Grafana

Login

↓

Dashboards

↓

Metrics

---

# Verify Logging

Open Grafana

↓

Explore

↓

Loki

↓

View Logs

---

# Verify Tracing

Grafana

↓

Tempo

↓

Traces

---

# Verify Alerts

Stop Application

↓

Prometheus Alert

↓

AlertManager

↓

Slack Notification

---

# Rollback

Jenkins

↓

Previous Build

↓

Redeploy

or

```bash
kubectl rollout undo deployment/hello-app
```

or

Git Revert

↓

ArgoCD Sync

---

# Common Commands

Pods

```bash
kubectl get pods -A
```

Logs

```bash
kubectl logs POD
```

Describe

```bash
kubectl describe pod POD
```

Restart

```bash
kubectl rollout restart deployment hello-app
```

Delete

```bash
kubectl delete pod POD
```

---

# Troubleshooting

## ImagePullBackOff

Check

- DockerHub credentials
- Image tag
- Registry

---

## CrashLoopBackOff

Check

```bash
kubectl logs POD
```

---

## ArgoCD OutOfSync

Sync

↓

Refresh

↓

Compare Git

---

## Trivy Failure

Review

```
reports/trivy-report.html
```

Fix vulnerabilities.

---

## Prometheus Missing Metrics

Check

ServiceMonitor

↓

Labels

↓

Endpoints

---

## Grafana No Logs

Verify

Promtail

↓

Loki

↓

Datasource

---

## Tempo No Traces

Verify

OpenTelemetry Collector

↓

Tempo

↓

Instrumentation

---

# Deployment Checklist

✅ GitHub Configured

✅ Jenkins Running

✅ Kubernetes Running

✅ Docker Registry Available

✅ Nexus Running

✅ ArgoCD Healthy

✅ Monitoring Installed

✅ Istio Enabled

✅ Trivy Working

✅ Ansible Working

✅ Slack Configured

✅ HPA Created

✅ Application Healthy

---

# Summary

The deployment process provisions an end-to-end DevSecOps platform that integrates CI/CD, GitOps, Kubernetes, Apache Tomcat, automated security scanning, infrastructure validation, observability, and service mesh capabilities. By following this guide, teams can consistently deploy the platform across Development, Staging, and Production environments while maintaining security, automation, and operational visibility.