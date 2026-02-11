# Day-03 – Kubernetes Learning

## 📚 Topics Covered
- Deployment
- ReplicaSet
- Scaling
- Service

---

## 🚀 Deployment

### What is a Deployment?

A **Deployment** is a Kubernetes resource used to manage applications declaratively.  
It ensures that a specified number of Pod replicas are running at all times.

Deployment automatically manages:
- ReplicaSets
- Pod creation
- Pod updates
- Rolling updates
- Rollbacks

---

## 🔁 ReplicaSet

### What is a ReplicaSet?

A **ReplicaSet** ensures that a specified number of Pod replicas are running at any given time.

When a Deployment is created:
- Kubernetes automatically creates a ReplicaSet
- The ReplicaSet creates and manages Pods

### Why do we use ReplicaSets?

- Maintain desired Pod count
- Self-healing (recreates Pods if deleted)
- Ensures high availability

---

## 📈 Scaling

### What is Scaling?

Scaling is the process of increasing or decreasing the number of running Pods in a Deployment.

### How Scaling Works

When we run:

```bash
kubectl scale deployment <deployment-name> --replicas=<number>
