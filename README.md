# Rolling Update in Kubernetes (k8s) for Two Versions of an App

This guide explains how to perform a rolling update in Kubernetes for two versions of your app. We'll use Docker to containerize the app and Kubernetes for orchestration.


https://github.com/mayaworld13/rolling_update_k8s/assets/127987256/a602f804-4d88-4329-b2de-92b8f893cde8


## Prerequisites

- Docker installed on your local machine
- Kubernetes cluster set up (e.g., Minikube, Docker Desktop with Kubernetes enabled, or a cloud provider like AWS EKS, Google GKE, or Azure AKS)
- Basic understanding of Docker and Kubernetes concepts

## Steps

### 1. Build Docker Images for Both Versions of the App

Assuming you have two versions of your app (`webserver:v1` and `webserver:v2`), build Docker images for both versions:

```bash
docker build -t webserver:v1 .
```
<p align="center">
  <img src="https://github.com/mayaworld13/rolling_update_k8s/assets/127987256/ed353a15-194c-42c1-995a-ab4701f6d2bc" alt="page" width="500" height="70" />
</p>


```bash
docker build -t webserver:v2 .
```
<p align="center">
  <img src="https://github.com/mayaworld13/rolling_update_k8s/assets/127987256/3d0455bb-9e0c-4b25-b1ea-d1a750e0d1ed" alt="page" width="480" height="50" />
</p>

### 2. Push Docker Images to a Container Registry

Push the Docker images to a container registry like Docker Hub:

```bash
docker tag webserver:v1 yourusername/webserver:v1
docker push yourusername/webserver:v1
docker tag webserver:v2 yourusername/webserver:v2
docker push yourusername/webserver:v2
```
---

## Deployment Management

### Creating a Deployment
To create a deployment, run the following command:

```bash
kubectl create deployment mydeploy --image=mayaworld13/webserver:v1
```

### Exposing the Deployment to public
Expose the deployment via a NodePort service using the following command:
```bash
kubectl expose deployment mydeploy --type=NodePort --port=8000
```
### Scaling the Deployment
The Deployment is scaled to multiple replicas using the kubectl scale command. This ensures high availability and scalability of the application.
Scale the deployment to multiple replicas using the following command:

```bash
kubectl scale deployment mydeploy --replicas=3
```

### Rollout Procedure
To perform a rolling update, the image of the Deployment is updated to the new version (`webserver:v2`) using the kubectl set image command.

To update the deployment image to a new version, use the following command:

```bash
kubectl set image deployment/mydeploy containername=mayaworld13/webserver:v2
```
you can get the container name `sudo crictl ps`
<p align="center">
  <img src="https://github.com/mayaworld13/rolling_update_k8s/assets/127987256/6256b933-64dc-4ced-baa2-644c93b18070" alt="page" width="1200" height="200" />
</p>




Verifying the Rollout
After updating the image, check the status of the pods to ensure the new version is running:

```bash
kubectl get pods
```
<p align="center">
  <img src="https://github.com/mayaworld13/rolling_update_k8s/assets/127987256/84458102-3eb3-4dc6-b58f-7c73d3af60a4" alt="page" width="700" height="200" />
</p>



### Monitoring Deployment
Monitor the rollout progress by watching the deployment status:

```bash
kubectl rollout status deployment/mydeploy
```


### Rollback Procedure
Rollback to Previous Version
If the updated version causes issues, rollback to the previous version using the following command:

```bash
kubectl rollout undo deployment/mydeploy
```
Verifying Rollback
After rolling back, verify that the deployment has reverted to the previous version:

```bash
kubectl get pods
```
Monitoring Rollback
Monitor the rollback progress by watching the deployment status:

```bash
kubectl rollout status deployment/mydeploy
```
