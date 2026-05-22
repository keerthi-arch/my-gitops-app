# Mini GitOps CI/CD Pipeline

A production-style GitOps pipeline deploying a Python Flask app 
to Google Kubernetes Engine using ArgoCD and Helm.

## Architecture
- **App**: Python Flask REST API containerized with Docker
- **Registry**: Docker Hub (effickeerthi/my-gitops-app)
- **Packaging**: Helm chart for Kubernetes deployment
- **Cluster**: Google Kubernetes Engine (GKE) — 2 node cluster
- **GitOps**: ArgoCD watches GitHub and auto-deploys on changes
- **Config repo**: Separate repo for deployment state (GitOps pattern)

## Repos
- `my-gitops-app` — application code + Helm chart
- `my-gitops-config` — deployment configuration (image tags)

## How it works
1. Code pushed to GitHub
2. Docker image built and pushed to Docker Hub
3. Image tag updated in config repo
4. ArgoCD detects change and syncs to GKE cluster
5. New pods rolled out automatically

## Tech stack
Docker · Kubernetes · Helm · ArgoCD · GKE · Python · Flask

