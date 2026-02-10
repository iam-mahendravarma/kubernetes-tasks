# Day 2 – Kubernetes Pods, Labels & Service Selectors

## 📌 Objective
Create multiple Kubernetes Pods with different `app` and `env` labels, and expose a specific Pod using a Service by matching labels through selectors.

This task demonstrates how **labels and selectors** help Kubernetes Services route traffic to the correct Pods.

---

## 🧱 Architecture Overview

- Two Pods are created:
  - **nginx Pod**
  - **jenkins Pod**
- Each Pod contains:
  - `app` label (application name)
  - `env` label (environment name)
- A Kubernetes **Service** is created to expose **only one Pod** by selecting matching labels.

---

## 📦 Pods Created

### 1️⃣ Nginx Pod
- Application: `nginx`
- Environment: `production`
- Port exposed: `80`
- Includes sample environment variables

### 2️⃣ Jenkins Pod
- Application: `jenkins`
- Environment: `development`
- Port exposed: `8080`
- Includes sample environment variables

---

## 🏷️ Labels Used

| Pod Name        | app     | env          |
|-----------------|---------|--------------|
| nginx-env-pod   | nginx   | production   |
| jenkins-env-pod | jenkins | development  |

Labels help Kubernetes identify and group resources logically.

---

## 🔌 Service Configuration

A Kubernetes **Service** is created using a selector:

```yaml
selector:
  app: nginx
  automate: jenkins
