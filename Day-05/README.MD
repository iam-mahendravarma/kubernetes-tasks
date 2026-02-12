Day-05 – Kubernetes Learning

Topics Covered

MySQL Pod Deployment

ClusterIP Service Configuration

CrashLoopBackOff Troubleshooting

Environment Variables in Containers

Internal Service Communication

Database Connectivity Testing

MySQL Pod Deployment
Objective

Deploy a MySQL 8.0 container inside Kubernetes and expose it internally using a ClusterIP Service.

Created two separate YAML files:

One for MySQL Pod

One for ClusterIP Service

Deployed the Pod using:

kubectl apply -f mysql-pod.yaml

Issue Faced – CrashLoopBackOff

After deployment, the Pod status showed:

CrashLoopBackOff


Verified the status:

kubectl get pods


Checked detailed information:

kubectl describe pod mysql-db-pod


Checked logs:

kubectl logs mysql-db-pod

Root Cause

The official MySQL 8.0 image requires a root password during initialization.

Since the environment variable for root password was not defined initially, the container exited with an error (Exit Code 1). Kubernetes restarted the container multiple times, resulting in CrashLoopBackOff.

Solution – Added Root Password

Updated the Pod configuration by adding the required environment variable for MySQL root password.

Recreated the Pod:

kubectl delete pod mysql-db-pod
kubectl apply -f mysql-pod.yaml


Verified the status:

kubectl get pods


Pod status changed to:

1/1 Running

ClusterIP Service Configuration

Created a ClusterIP Service to expose MySQL internally within the cluster.

Applied the Service:

kubectl apply -f mysql-service.yaml


Verified Service creation:

kubectl get svc


Confirmed that the Service type is:

ClusterIP

Testing Database Connectivity

Since ClusterIP is accessible only inside the cluster, created a temporary MySQL client Pod.

Command used:

kubectl run mysql-client --rm -it --image=mysql:8.0 -- bash


Inside the client Pod, connected to MySQL using:

mysql -h mysql-service -u root -p


After entering the root password, successfully accessed the MySQL shell.

Internal Communication Flow

MySQL Client Pod → ClusterIP Service → MySQL Pod

Pod Label: db=mysql

Service Selector: db=mysql

Access Method: mysql-service:3306

Communication happens through internal Kubernetes DNS

Key Learnings

MySQL container requires mandatory environment variables during initialization.

Missing required variables can cause CrashLoopBackOff.

ClusterIP Services enable internal communication within the cluster.

Service name acts as internal DNS for communication.

Labels and Selectors must match for proper traffic routing.

Production Best Practices

Use Kubernetes Secrets instead of plain text passwords.

Use Persistent Volumes for database storage.

Deploy MySQL using Deployment or StatefulSet for better management.

Add resource requests and limits.

Avoid running databases as a single standalone Pod in production.

Status
Component	Status
MySQL Pod	✅ Running
ClusterIP Service	✅ Created
DB Connectivity	✅ Verified