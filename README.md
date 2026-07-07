# HTML GitOps Demo using Argo CD

## Overview

This project demonstrates an end-to-end GitOps workflow using **Argo CD** on **Docker Desktop Kubernetes**. It deploys two independent HTML applications (**Frontend** and **Backend**) and manages them declaratively through a GitHub repository.

The project showcases several Argo CD features, including:

* GitOps-based deployments
* ApplicationSet
* Automated Sync
* Self-Healing
* Pruning
* ConfigMaps
* Secrets
* Horizontal Pod Autoscaler (HPA)
* Health Checks
* Rollback using Git
* Kustomize

---

# Architecture

```text
                     GitHub Repository
                            │
                            ▼
                     Argo CD ApplicationSet
                            │
            ┌───────────────┴───────────────┐
            ▼                               ▼
      Frontend Application           Backend Application
            │                               │
            ▼                               ▼
      Frontend Namespace            Backend Namespace
            │                               │
      ├── Deployment                 ├── Deployment
      ├── Service                    ├── Service
      ├── ConfigMap                  ├── ConfigMap
      ├── Secret                     ├── Secret
      └── HPA                        └── HPA
```

---

# Project Structure

```text
html-gitops-demo/
│
├── applicationsets/
│   └── applicationset.yaml
│
├── apps/
│   ├── frontend/
│   │   ├── namespace.yaml
│   │   ├── configmap.yaml
│   │   ├── secret.yaml
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   ├── hpa.yaml
│   │   └── kustomization.yaml
│   │
│   └── backend/
│       ├── namespace.yaml
│       ├── configmap.yaml
│       ├── secret.yaml
│       ├── deployment.yaml
│       ├── service.yaml
│       ├── hpa.yaml
│       └── kustomization.yaml
│
└── README.md
```

---

# Prerequisites

* Docker Desktop
* Kubernetes enabled in Docker Desktop
* kubectl
* Git
* GitHub Account
* Argo CD installed on Kubernetes

---

# Verify Kubernetes

```bash
kubectl get nodes
```

Expected output:

```text
NAME               STATUS   ROLES           AGE
docker-desktop     Ready    control-plane
```

---

# Install Argo CD

Create the namespace:

```bash
kubectl create namespace argocd
```

Install Argo CD:

```bash
kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Verify:

```bash
kubectl get pods -n argocd
```

---

# Access Argo CD

Start port forwarding:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Open:

```
https://localhost:8080
```

Retrieve the admin password:

```bash
kubectl get secret argocd-initial-admin-secret \
-n argocd \
-o jsonpath="{.data.password}" | base64 --decode
```

Login using:

* Username: **admin**
* Password: **<decoded password>**

---

# Deploy the Applications

Apply the ApplicationSet:

```bash
kubectl apply -f applicationsets/applicationset.yaml
```

Verify the Applications:

```bash
kubectl get applications -n argocd
```

Verify the workloads:

```bash
kubectl get all -A
```

---

# Access the Applications

Frontend:

```bash
kubectl port-forward svc/frontend-service -n frontend 8081:80
```

Open:

```
http://localhost:8081
```

Backend:

```bash
kubectl port-forward svc/backend-service -n backend 8082:80
```

Open:

```
http://localhost:8082
```

---

# GitOps Workflow

1. Modify the Kubernetes manifests or application content.
2. Commit the changes.
3. Push the changes to GitHub.
4. Argo CD detects the changes.
5. Argo CD synchronizes the cluster automatically.
6. Kubernetes updates the running application.

---

# Demonstrating an Update

Update the HTML content inside the ConfigMap.

Commit the changes:

```bash
git add .
git commit -m "Updated frontend to version 2"
git push origin main
```

Argo CD automatically synchronizes the changes.

---

# Demonstrating a Rollback

Revert the previous commit:

```bash
git revert HEAD
git push origin main
```

Argo CD detects the Git change and restores the previous version.

---

# Features Demonstrated

* GitOps Deployment
* Argo CD ApplicationSet
* Automated Synchronization
* Self-Healing
* Pruning
* ConfigMaps
* Secrets
* Kubernetes Deployments
* Services
* Horizontal Pod Autoscaler (HPA)
* Health Monitoring
* Git-Based Rollback
* Docker Desktop Kubernetes
* Kustomize

---

# Learning Objectives

By completing this project, you will understand how to:

* Deploy applications using Argo CD.
* Implement GitOps workflows.
* Organize Kubernetes manifests using Kustomize.
* Manage multiple applications using an ApplicationSet.
* Configure ConfigMaps and Secrets.
* Enable automatic synchronization and self-healing.
* Perform application updates through Git commits.
* Roll back deployments using Git history.
* Deploy applications on Docker Desktop Kubernetes.

---

# Author

**Nagaraju Thota**

GitOps Demo Project using Argo CD, Docker Desktop Kubernetes, and GitHub.
