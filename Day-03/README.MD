# Kubernetes Deployment, Scaling, and Service Exposure

## 📌 Overview
This task demonstrates how to:
- Create a Kubernetes Deployment
- Verify ReplicaSets and Pods
- Scale the Deployment using a command
- Create a Service to expose the application
- Verify the application output

---

## 🛠️ Step 1: Create Deployment

Created a `deployment.yaml` file and applied it:

```bash
kubectl apply -f deployment.yaml
🔍 Verify Deployment, ReplicaSet, and Pods
bash
Copy code
kubectl get deployments
kubectl get rs
kubectl get pods
✅ Observation
Deployment was created successfully.

A ReplicaSet was automatically created.

Pods were created based on the replica count defined in the YAML file.

📈 Step 2: Scale the Deployment
Scaled the Deployment using the following command:

bash
Copy code
kubectl scale deployment <deployment-name> --replicas=<number>
🔹 Example
bash
Copy code
kubectl scale deployment nginx-deployment --replicas=5
🔍 Verify Scaling
bash
Copy code
kubectl get pods
kubectl get rs
✅ Observation
New Pods were created automatically.

ReplicaSet updated the desired replica count.

Scaling happened without downtime.

🌐 Step 3: Create Service to Expose Deployment
Created a service.yaml file to expose the Deployment.

bash
Copy code
kubectl apply -f service.yaml
🔍 Verify Service
bash
Copy code
kubectl get svc