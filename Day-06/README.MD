# Day-06 – Kubernetes Learning

## 📘 Topics Covered
- Jenkins Deployment using Deployment
- NodePort Service Configuration
- Accessing Jenkins UI via Browser
- Retrieving Initial Admin Password
- Internal Service Communication Flow
- Best Practices for Jenkins in Kubernetes

---

## 🎯 Objective
Deploy **Jenkins (LTS)** inside Kubernetes using a **Deployment** and expose it externally via a **NodePort Service** so the Jenkins UI can be accessed from a browser.

---

## 🐳 Jenkins Deployment
- Created two YAML files:
  - `jenkins-deployment.yaml` → Jenkins Deployment
  - `jenkins-service.yaml` → NodePort Service
- Deployed Jenkins:
  ```bash
  kubectl apply -f jenkins-deployment.yaml

##  NodePort Service Configuration

- Created a NodePort Service (jenkins-service.yaml).
- Applied:
kubectl apply -f jenkins-service.yaml

## Accessing Jenkins UI

- Get Node IP:
kubectl get nodes -o wide
- Open Jenkins in browser:
http://<NodeIP>:30080

## Retrieve Initial Admin Password
kubectl exec -it deploy/jenkins-deployment -n jenkins -- cat /var/jenkins_home/secrets/initialAdminPassword

- Copy the password → Paste into Jenkins setup wizard.








