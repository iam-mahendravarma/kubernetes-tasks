# 🔐 Amazon EKS – IAM User Access via aws-auth ConfigMap

## 📌 Overview

This document explains how IAM user access was granted to an Amazon EKS cluster by modifying the `aws-auth` ConfigMap in the `kube-system` namespace.

IAM User **varma** was mapped to Kubernetes RBAC with full administrative privileges.

---

## 🏗️ How EKS Authentication Works

Amazon EKS uses the `aws-auth` ConfigMap to map:

- IAM Users → Kubernetes Users  
- IAM Roles → Kubernetes Roles  
- IAM Entities → Kubernetes RBAC Groups  

Only IAM identities defined in this ConfigMap can authenticate and access the cluster using `kubectl`.

---

## 🛠️ Steps Performed

### 1️⃣ Update kubeconfig (Admin Access)

```bash
aws eks update-kubeconfig --region ap-south-1 --name <cluster-name>
```

---

### 2️⃣ Verify Cluster Access

```bash
kubectl get nodes
```

---

### 3️⃣ Check Existing aws-auth ConfigMap

```bash
kubectl get configmap aws-auth -n kube-system -o yaml
```

---

### 4️⃣ Edit aws-auth ConfigMap

```bash
kubectl edit configmap aws-auth -n kube-system
```

Inside the `mapUsers` section, add the IAM user and assign:

```
groups:
  - system:masters
```

Save and exit the editor.

---

## 🔑 Permission Details

| Field | Description |
|-------|------------|
| `userarn` | IAM User ARN |
| `username` | Kubernetes username |
| `system:masters` | Grants full cluster-admin privileges |

⚠️ `system:masters` provides complete administrative control over the cluster.

---

## ✅ Verification Steps

### Configure IAM User Profile

```bash
aws configure --profile varma
```

---

### Update kubeconfig Using IAM User

```bash
aws eks update-kubeconfig --region ap-south-1 --name <cluster-name> --profile varma
```

---

### Test Access

```bash
kubectl get nodes
kubectl get pods -A
kubectl create namespace test-access
```

If the commands execute successfully without `Forbidden` errors, IAM user access is confirmed.

---

## 🔐 Production Best Practices

- Avoid granting `system:masters` unless absolutely necessary  
- Use namespace-scoped RBAC roles (`view`, `edit`)  
- Prefer IAM Roles over IAM Users  
- Consider using EKS Access Entries (recommended modern approach)  
- Follow the Principle of Least Privilege  

---

## 📚 Key Learnings

- Understanding aws-auth ConfigMap structure  
- Mapping IAM identities to Kubernetes RBAC  
- Managing cluster-level access securely  
- Verifying IAM-based authentication in EKS  

---

## 🎯 Outcome

✅ Successfully configured IAM user access to the EKS cluster using aws-auth ConfigMap.

---

## 🏷️ Tags

`AWS` `EKS` `Kubernetes` `IAM` `DevOps` `RBAC` `CloudSecurity`