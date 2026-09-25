# cicd-nginx-demo
exercising ci-cd

# CI/CD Nginx Demo

A small CI/CD project demonstrating automatic Docker image builds and Kubernetes deployments using GitHub Actions.

## Architecture

Developer
   ↓
GitHub
   ↓
GitHub Actions
   ↓
Docker Build
   ↓
GitHub Container Registry
   ↓
Self-hosted GitHub Runner
   ↓
Kubernetes
   ↓
Nginx Pods
   ↓
NodePort Service

## Technologies

- Docker
- Kubernetes
- GitHub Actions
- GitHub Container Registry
- AWS EC2
- Nginx
- Linux

## Kubernetes Architecture

The cluster contains:

- 1 control-plane node
- 2 worker nodes
- 1 Deployment
- 2 Nginx replicas
- 1 NodePort Service

The Deployment uses the Docker image:

ghcr.io/florinmnt/cicd-nginx-demo:latest

## CI/CD Pipeline

When code is pushed to the `main` branch:

1. GitHub Actions starts automatically.
2. The repository is checked out.
3. A Docker image is built.
4. The image is pushed to GitHub Container Registry.
5. The deployment job runs on a self-hosted runner installed on the Kubernetes control-plane node.
6. The runner executes:

   kubectl rollout restart deployment/cicd-nginx-demo

7. Kubernetes performs a rolling update.
8. The new application version becomes available through the NodePort service.

## GitHub Actions

The workflow contains two jobs:

### Build

Runs on a GitHub-hosted runner:

ubuntu-latest

Responsibilities:

- build Docker image
- authenticate to GHCR
- push image

### Deploy

Runs on:

self-hosted

Responsibilities:

- connect to the Kubernetes cluster
- restart the Deployment
- wait for the rollout to finish

The deploy job only runs if the build job succeeds.

## Deployment

Kubernetes runs two replicas:

kubectl get pods -l app=cicd-nginx-demo

The application is exposed using a NodePort service on:

30080

## What I learned

This project helped me understand:

- Docker images and containers
- container registries
- Kubernetes Deployments
- ReplicaSets and Pods
- Kubernetes Services and NodePort
- rolling updates
- GitHub Actions workflows
- CI vs CD
- self-hosted GitHub Actions runners
- automated Kubernetes deployments
