# 🔐 AWS EKS – IAM User Access via aws-auth ConfigMap

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

### 1️⃣ Update kubeconfig

```bash
aws eks update-kubeconfig --region ap-south-1 --name <cluster-name>
```

---

### 2️⃣ Check Existing aws-auth ConfigMap

```bash
kubectl get configmap aws-auth -n kube-system -o yaml
```

---

### 3️⃣ Edit aws-auth ConfigMap

```bash
kubectl edit configmap aws-auth -n kube-system
```

---

## 📂 Final aws-auth ConfigMap Configuration

```yaml
apiVersion: v1
data:
  mapRoles: |
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::992382429239:role/eks-node-group-role
      username: system:node:{{EC2PrivateDNSName}}

  mapUsers: |
    - userarn: arn:aws:iam::789094741120:user/varma
      username: varma
      groups:
        - system:masters

kind: ConfigMap
metadata:
  name: aws-auth
  namespace: kube-system
```

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
```

If the commands execute successfully, IAM user access is confirmed.

---

## 🔐 Production Best Practices

- Avoid granting `system:masters` unless necessary
- Use namespace-scoped RBAC roles (`view`, `edit`)
- Prefer IAM Roles over IAM Users
- Consider using EKS Access Entries (modern recommended method)

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
