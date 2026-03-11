# Deployment on Kubernetes 

Minikube used to run Kubernetes on local machine and nginx image as sample image to containerize.

This project demonstrates how to **deploy and run an Nginx container on a local Kubernetes cluster using Minikube**.  
It includes installation steps, cluster setup, deployment configuration, and service exposure.

---

# Architecture

```
Local Machine
     │
     ▼
 Minikube Cluster
     │
     ▼
 Kubernetes Deployment
     │
     ▼
 Nginx Pod
     │
     ▼
 Kubernetes Service
     │
     ▼
 Browser Access
```

---

# Prerequisites

Make sure the following tools are installed:

- Docker
- kubectl
- Minikube

Check installation:

```bash
docker --version
kubectl version --client
minikube version
```

# Start Minikube Cluster

Start the Kubernetes cluster:

```bash
minikube start --driver=docker
```

Check cluster status:

```bash
minikube status
```

Verify nodes:

```bash
kubectl get nodes
```

Expected output:

```
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   1m    v1.xx.x
```

---

# Pull Nginx Docker Image

Pull the official Nginx image from Docker Hub:

```bash
docker pull nginx
```

Verify image:

```bash
docker images
```

---

# Create Kubernetes Deployment

Create a file named:

```
nginx-deployment.yaml
```

Add the following configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        resources:
          requests:
              memory: "64Mi"
              cpu: "100m"
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 80
```

---

# Deploy Nginx

Apply the deployment file:

```bash
kubectl apply -f nginx-deployment.yaml
```

Check deployment:

```bash
kubectl get deployments
```

Check pods:

```bash
kubectl get pods
```
Example:

<img width="568" height="174" alt="image" src="https://github.com/user-attachments/assets/5f78d997-cc68-4534-ace0-8d5348b3b7e4" />


---

# Expose Deployment

Expose the deployment as a NodePort service:

```bash
kubectl expose deployment nginx-deployment --type=NodePort --port=80
```

Check service:

```bash
kubectl get svc
```

Example:

<img width="766" height="131" alt="image" src="https://github.com/user-attachments/assets/cc6e81b8-c868-4a60-8270-e2ad6c3e8c58" />


---

# Access Nginx

Run:

```bash
minikube service nginx-deployment
```

<img width="754" height="288" alt="image" src="https://github.com/user-attachments/assets/e2c5fc72-91c1-4d3d-bc77-19db58a687c4" />


This will open the **Nginx Welcome Page** in your browser.

<img width="1366" height="726" alt="image" src="https://github.com/user-attachments/assets/f5fce47f-9bb4-4a39-993e-e2ee2d75c296" />


---

# Open Kubernetes Dashboard

Start the dashboard:

```bash
minikube dashboard
```

This opens the **Kubernetes Web UI** where you can monitor:

- Nodes
- Pods
- Deployments
- Services
- Cluster resources

<img width="1364" height="724" alt="image" src="https://github.com/user-attachments/assets/77fac30a-facb-4421-a861-bd611bb4b1e9" />


---

# Useful Kubernetes Commands

Cluster information:

```bash
kubectl cluster-info
```

List pods:

```bash
kubectl get pods
```

Describe pod:

```bash
kubectl describe pod <pod-name>
```

View logs:

```bash
kubectl logs <pod-name>
```

Delete deployment:

```bash
kubectl delete deployment nginx-deployment
```

Delete service:

```bash
kubectl delete svc nginx-deployment
```

Stop Minikube:

```bash
minikube stop
```

Delete cluster:

```bash
minikube delete
```

---

# Repository Structure

```
kubernetes-deployment-demo
│
├── nginx-deployment.yaml
└── README.md
```

---

# Learning Outcomes

After completing this project you will learn:

- Kubernetes basics
- Running Kubernetes locally with Minikube
- Creating deployments
- Managing pods
- Exposing services
- Using Kubernetes dashboard

---
