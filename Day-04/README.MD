# Day-04 – Kubernetes Learning

## Topics Covered
- Recreate Deployment Strategy
- RollingUpdate Deployment Strategy
- Image Update (Jenkins Old Version → New Version)
- Downtime Analysis

---

## Recreate Deployment Strategy

### What is Recreate Strategy?

The **Recreate** strategy terminates all existing Pods before creating new Pods with the updated configuration or image.

### How It Works

- All old Pods are stopped.
- After termination, new Pods are created with the updated image.
- There is a temporary downtime during the transition.

### Example Scenario

- Created a Deployment using **Recreate** strategy.
- Updated the Jenkins image from an old version to a new version.
- Observed that:
  - All old Pods were terminated first.
  - New Pods were created afterward.
  - Application was unavailable during this period.

### Disadvantage

- Causes **downtime**
- Not suitable for production environments requiring high availability

---

## RollingUpdate Deployment Strategy

### What is RollingUpdate Strategy?

The **RollingUpdate** strategy updates Pods gradually without stopping the entire application.

It replaces old Pods with new Pods one by one.

### How It Works

- A new Pod is created with the updated image.
- Once it becomes ready, one old Pod is terminated.
- This process continues until all Pods are updated.
- Application remains available during the update.

### Example Scenario

- Created a Deployment using **RollingUpdate** strategy.
- Updated Jenkins image from old version to new version.
- Observed that:
  - Pods were updated one by one.
  - No complete downtime occurred.
  - Application remained accessible.

---

## Image Update Process

Updated the Jenkins image in the Deployment YAML file:

```yaml
image: jenkins/jenkins:<new-version>

```bash
kubectl apply -f deployment.yaml

## Monitored the rollout:

kubectl rollout status deployment <deployment-name>
kubectl get pods -w

| Strategy      | Downtime | Production Suitable |
| ------------- | -------- | ------------------- |
| Recreate      | Yes      | ❌ No                |
| RollingUpdate | No       | ✅ Yes               |

