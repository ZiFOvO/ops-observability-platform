# Ops Observability Platform

OpsScope is a secure, tunnel-only Kubernetes-based platform built on k3s. It provides centralized observability, operational visibility, and controlled access to platform services using production-style DevOps patterns.

This project demonstrates infrastructure-as-code delivery, secure ingress design, observability integration, and CI-driven deployment workflows.

---

## Objectives

- Build a production-style DevOps control dashboard
- Centralize metrics, logs, and alerts
- Implement secure, tunnel-only ingress architecture
- Manage infrastructure declaratively via Git
- Automate deployment through CI/CD

---

## Architecture Overview

### Environment

- Ubuntu
- k3s
- Traefik
- Cloudflare Tunnel
- Cloudflare Access
- cert-manager

---

### High-Level Traffic Flow

User
└── Cloudflare Access (authentication)
    └── Cloudflare Tunnel
        └── Traefik Ingress
            └── Kubernetes Service
                └── Pod

No public NodePorts.  
No router NAT forwarding.  
All services are tunnel-only.

---

## Platform Components

### Observability

Installed via Helm:

- Prometheus
- Grafana
- Alertmanager
- Node exporter
- kube-state-metrics

Features:
- Cluster health dashboard
- Node readiness monitoring
- Deployment availability tracking
- CrashLoopBackOff detection
- Firing alert visibility

---

### Logging

Installed via Helm:

- Loki
- Promtail (collects pod logs)

---

### Portal

Homepage dashboard provides:

- Kubernetes cluster widget (CPU, memory, pods, nodes)
- Observability links (Grafana, Prometheus, Alertmanager, Loki)
- Operational shortcuts
- Runbook references

Dashboard layout as ConfigMap.

---

## Security Model

- All subdomains protected by Cloudflare Access
- No direct public exposure
- No LoadBalancer services
- No NodePort exposure
- TLS certificates issued via cert-manager
- Grafana embedding disabled
- No anonymous access enabled

Architecture is tunnel-only by design.

---

## CI/CD Workflow

- GitHub self-hosted runner runs inside the platform
- On push to `main`:
  - `kubectl apply` for manifests
  - `helm upgrade --install` for monitoring and logging stacks
  - Basic smoke checks

Infrastructure is version-controlled.

---

## Repository Structure

ops-observability-platform
├── apps
├── clusters
│   └── dev
│       ├── cert-manager
│       │   └── clusterissuer-letsencrypt-cloudflare.yaml
│       ├── observability
│       │   ├── dashboards
│       │   │   └── dashboard-1771373357768.json
│       │   ├── helm-values
│       │   │   ├── kube-prometheus-stack.values.yaml
│       │   │   └── loki-stack.values.yaml
│       │   ├── ingress
│       │   │   ├── alertmanager-ingress.yaml
│       │   │   ├── grafana-ingress.yaml
│       │   │   ├── grafana-loki-datasource.yaml
│       │   │   ├── loki-ingress.yaml
│       │   │   └── prometheus-ingress.yaml
│       │   ├── rules
│       │   │   └── platform-alerts.yaml
│       │   └── runbooks
│       │       └── README.md
│       ├── portal
│       │   └── homepage
│       │       ├── deploy-service.yaml
│       │       ├── homepage-config.yaml
│       │       ├── homepage-ingress.yaml
│       │       └── homepage-rbac.yaml
│       └── smoke-tests
│           └── whoami
│               ├── whoami-app.yaml
│               └── whoami-ingress.yaml
├── README.md
└── scripts

---

## Public Endpoints (Access-Protected)

- https://opsscope.dev
- https://grafana.opsscope.dev
- https://prometheus.opsscope.dev
- https://alertmanager.opsscope.dev
- https://loki.opsscope.dev

All subdomains require authentication.

---

## Current Status

- Fully functional observability stack
- Secure ingress architecture
- CI-managed deployments
- Version-controlled configuration
- Production-style access model

---

## Why This Project Matters

This project demonstrates:

- Kubernetes platform design
- Secure ingress architecture
- Observability stack integration
- DevOps CI/CD workflows
- Infrastructure-as-Code principles
