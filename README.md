# Ops Observability Platform

A production-style **Ops / Observability platform** built on Kubernetes (k3s) to monitor
cluster health, workloads, deployments, and operational signals.

This project focuses on **real DevOps tooling and workflows**: ingress, metrics, logs,
alerting, CI/CD visibility, and platform delivery patterns.

---

## 🎯 Goals

- Build a realistic **Ops Console / Service Catalog**
- Observe Kubernetes **runtime health**
- Centralize **metrics, logs, and alerts**
- Practice **production-grade DevOps patterns**
- Treat infrastructure as a **versioned platform**

---

## 🧱 Architecture Overview

### High-Level Traffic Flow

```text
User / Browser
      |
      v
Ingress Controller
      |
      v
NGINX App Gateway
  ├─ Serves Ops Console UI (static)
  └─ Proxies /api requests
          |
          v
Platform Services
  ├─ Kubernetes API (cluster state)
  ├─ Prometheus (metrics)
  ├─ Loki (logs)
  ├─ Alertmanager (alerts)
  └─ CI/CD metadata
