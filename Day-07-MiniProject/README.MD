# 📝 Todo Application – Kubernetes 3-Tier Deployment

This project demonstrates deployment of a **3-Tier Todo Application** using Kubernetes.

- 🖥 Frontend: Express (Node.js)
- 🐍 Backend: Python API
- 🗄 Database: MongoDB
- ☸ Orchestration: Kubernetes
- 📦 Containerization: Docker

---

# 🚀 Architecture

User
↓
NodePort Service (todo-frontend-service : 30009)
↓
Frontend Pod (Express - Port 3000)
↓
ClusterIP Service (todo-backend-service : 8000)
↓
Backend Pod (Python API - Port 8000)
↓
ClusterIP Service (mongodb-service : 27017)
↓
MongoDB Pods


---

# 🏗 Kubernetes Configuration Overview

## 🔹 1️⃣ Backend (Python API)

### Deployment Details

- Name: `todo-backend-deployment`
- Replicas: 3
- Strategy: `Recreate`
- Container Port: 8000
- Image: `iammahendravarma20/todoappbackend:latest`

### Service Details

- Name: `todo-backend-service`
- Type: `ClusterIP`
- Port: 8000
- Internal access only

### Internal Backend Access

Frontend connects using:

http://todo-backend-service:8000


---

## 🔹 2️⃣ Frontend (Express)

### Deployment Details

- Name: `todo-frontend-deployment`
- Replicas: 3
- Strategy: `Recreate`
- Container Port: 3000
- Image: `iammahendravarma20/todoappfrontend:latest`

### Service Details

- Name: `todo-frontend-service`
- Type: `NodePort`
- Service Port: 80
- Target Port: 3000
- NodePort: `30009`

### External Access

http://<NodeIP>:30009


---

## 🔹 3️⃣ MongoDB

### Deployment Details

- Name: `mongodb-deployment`
- Replicas: 3
- Strategy: `Recreate`
- Container Port: 27017
- Image: `mongo:latest`

### Service Details

- Name: `mongodb-service`
- Type: `ClusterIP`
- Port: 27017

### Backend MongoDB Connection String

mongodb://mongodb-service:27017


---

# ⚙️ Deployment Strategy Used

All deployments use:

strategy:
type: Recreate


### What This Means:

- Old pods are terminated first
- New pods are created after
- Temporary downtime occurs during updates

---

# 📦 Deployment Commands

Apply all resources:

```bash
kubectl apply -f backend.yaml
kubectl apply -f frontend.yaml
kubectl apply -f mongodb.yaml

Check Pods:

kubectl get pods

Check Services:

kubectl get svc

