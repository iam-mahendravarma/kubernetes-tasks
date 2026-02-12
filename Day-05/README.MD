Here’s a polished **README.md** version of your Day-05 Kubernetes learning notes:

```markdown
# Day-05 – Kubernetes Learning

## 📘 Topics Covered
- MySQL Pod Deployment
- ClusterIP Service Configuration
- CrashLoopBackOff Troubleshooting
- Environment Variables in Containers
- Internal Service Communication
- Database Connectivity Testing

---

## 🎯 Objective
Deploy a **MySQL 8.0 container** inside Kubernetes and expose it internally using a **ClusterIP Service**.

---

## 🐳 MySQL Pod Deployment
- Created two separate YAML files:
  - `mysql-pod.yaml` → MySQL Pod
  - `mysql-service.yaml` → ClusterIP Service
- Deployed the Pod:
  ```bash
  kubectl apply -f mysql-pod.yaml
  ```

### ❌ Issue Faced – CrashLoopBackOff
- Pod status: `CrashLoopBackOff`
- Verified with:
  ```bash
  kubectl get pods
  kubectl describe pod mysql-db-pod
  kubectl logs mysql-db-pod
  ```
- **Root Cause:**  
  The official MySQL 8.0 image requires a **root password** during initialization.  
  Missing environment variable → container exited with error (Exit Code 1) → Kubernetes restarted repeatedly → CrashLoopBackOff.

### ✅ Solution – Added Root Password
- Updated Pod configuration with required environment variable:
  ```yaml
  env:
    - name: MYSQL_ROOT_PASSWORD
      value: my-secret-pw
  ```
- Recreated Pod:
  ```bash
  kubectl delete pod mysql-db-pod
  kubectl apply -f mysql-pod.yaml
  ```
- Verified:
  ```bash
  kubectl get pods
  ```
- Status: `1/1 Running`

---

## 🌐 ClusterIP Service Configuration
- Created a ClusterIP Service (`mysql-service.yaml`).
- Applied:
  ```bash
  kubectl apply -f mysql-service.yaml
  ```
- Verified:
  ```bash
  kubectl get svc
  ```
- Service type: **ClusterIP**

---

## 🔍 Testing Database Connectivity
- Created a temporary MySQL client Pod:
  ```bash
  kubectl run mysql-client --rm -it --image=mysql:8.0 -- bash
  ```
- Connected to MySQL:
  ```bash
  mysql -h mysql-service -u root -p
  ```
- Entered root password → Successfully accessed MySQL shell.

---

## 🔄 Internal Communication Flow
```
MySQL Client Pod → ClusterIP Service → MySQL Pod
```

- **Pod Label:** `db=mysql`  
- **Service Selector:** `db=mysql`  
- **Access Method:** `mysql-service:3306`  
- Communication via **internal Kubernetes DNS**

---

## 📚 Key Learnings
- MySQL container requires mandatory environment variables during initialization.
- Missing required variables → CrashLoopBackOff.
- ClusterIP Services enable internal communication within the cluster.
- Service name acts as internal DNS.
- Labels and selectors must match for proper traffic routing.

---

## 🛡️ Production Best Practices
- Use **Kubernetes Secrets** instead of plain text passwords.
- Use **Persistent Volumes** for database storage.
- Deploy MySQL using **Deployment** or **StatefulSet** for better management.
- Add **resource requests and limits**.
- Avoid running databases as a single standalone Pod in production.

---

## ✅ Status
| Component        | Status       |
|------------------|--------------|
| MySQL Pod        | ✅ Running   |
| ClusterIP Service| ✅ Created   |
| DB Connectivity  | ✅ Verified  |
```

