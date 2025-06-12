# Infrastructure Templates with Kustomize and Helm

This repository contains Kubernetes manifests managed with **Kustomize** and **Helm** for multiple environments running on **Azure Kubernetes Service (AKS)**.

## Directory Structure

```
helm/                   # Generic Helm chart for microservices
kustomize/
  base/                 # Base manifests
  overlays/
    dev/
    staging/
    prod-us-east/
    prod-us-west/
.github/workflows/      # CI/CD pipelines
```

## Environments

- **dev** – always deploys the `latest` image tag and uses `imagePullPolicy: Always` so the newest image is pulled whenever the deployment restarts.
- **staging** – uses the image tag specified in the manifest.
- **prod-us-east** – uses the image tag specified in the manifest.
- **prod-us-west** – uses the image tag specified in the manifest.

## GitHub Actions Workflows

- **build-and-push.yml** – builds Docker images and pushes them to Azure Container Registry (ACR).
- **deploy.yml** – applies Kustomize overlays and Helm charts to the AKS cluster.

## Required GitHub Secrets

The following secrets should be created in the repository settings:

| Secret | Purpose |
| ------ | ------- |
| `AZURE_CREDENTIALS` | Service principal credentials in JSON used by `azure/login`. |
| `ACR_LOGIN_SERVER` | Login server of the ACR instance (e.g. `myregistry.azurecr.io`). |
| `AZURE_CONTAINER_REGISTRY_USERNAME` | Username for ACR. |
| `AZURE_CONTAINER_REGISTRY_PASSWORD` | Password for ACR. |
| `AKS_CLUSTER_NAME` | Name of the AKS cluster. |
| `AKS_RESOURCE_GROUP` | Azure resource group containing the AKS cluster. |

These secrets allow the workflows to authenticate with Azure, push container images, and deploy manifests.

