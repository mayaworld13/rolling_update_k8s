# Rolling Update in Kubernetes (k8s) for Two Versions of an App

This guide explains how to perform a rolling update in Kubernetes for two versions of your app. We'll use Docker to containerize the app and Kubernetes for orchestration.

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


