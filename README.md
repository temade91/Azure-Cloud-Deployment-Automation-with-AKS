# Azure Cloud Deployment Automation with AKS

## Project Overview

This project demonstrates the containerisation and deployment of a Netflix Clone web application using Docker, Kubernetes, Azure Kubernetes Service (AKS), and GitHub Actions.

The application is served using Nginx and packaged into a Docker image. GitHub Actions automates the image build and publication to Docker Hub, followed by deployment to AKS.

The deployment workflow supports two environments:

- **Development:** `release` branch → `dev` namespace
- **Production:** `master` branch → `prod` namespace

## Technologies Used

| Technology | Purpose |
|---|---|
| Azure Kubernetes Service (AKS) | Managed Kubernetes environment |
| Docker | Application containerisation |
| Nginx | Serving the static web application |
| Docker Hub | Container image storage |
| Kubernetes | Application deployment and service management |
| GitHub Actions | CI/CD automation |
| Azure CLI | Azure resource management |
| kubectl | Kubernetes resource management |

## Architecture and Deployment Flow

The GitHub Actions workflow automates the following process:

1. A push to the `master` or `release` branch triggers the workflow.
2. GitHub Actions checks out the repository.
3. Docker Buildx builds the application image.
4. The image is pushed to Docker Hub using `latest` and commit-SHA tags.
5. The Kubernetes deployment manifest is updated with the commit-specific image.
6. The workflow selects the deployment namespace based on the branch.
7. GitHub Actions authenticates with Azure using GitHub Secrets.
8. AKS credentials are retrieved using Azure CLI.
9. Kubernetes applies the deployment and service manifests to the selected namespace.
10. The workflow checks rollout status and retrieves service information.

## Containerisation

The application uses the official `nginx:alpine` image as its base.

The Dockerfile removes the default Nginx web content, copies the application files into the Nginx web directory, exposes port `80`, and starts Nginx in the foreground.

This provides a lightweight container for serving the static web application.

## Kubernetes Configuration

The Kubernetes manifests are stored in the `k8-manifest` directory.

### Deployment

The deployment manifest defines:

- **Name:** `netflix-clone-deployment`
- **Replicas:** 2
- **Container port:** 80
- **CPU request:** 250m
- **CPU limit:** 500m
- **Memory request:** 256Mi
- **Memory limit:** 512Mi

The workflow updates the container image reference to use the commit-SHA tag before applying the manifest.

### Service

The service manifest defines:

- **Name:** `netflix-clone-service`
- **Type:** `LoadBalancer`
- **Protocol:** TCP
- **Port:** 80
- **Target port:** 80

The LoadBalancer configuration provides an external access point through the cloud load balancer.

## GitHub Repository Templates

The repository includes issue and pull request templates to provide structure for collaboration.

**Bug report template**
- Bug description
- Steps to reproduce
- Expected behaviour
- Additional context or screenshots

**Pull request template**
- Feature testing checklist
- Changelog and conflict checks
- Stakeholder approval checklist
- Database migration and release-readiness questions
- Summary of proposed changes

## Secrets and Configuration

The GitHub Actions workflow uses the following repository secrets:

| Secret | Purpose |
|---|---|
| `DOCKERHUB_USERNAME` | Docker Hub username |
| `DOCKERHUB_TOKEN` | Docker Hub authentication |
| `AZURE_CREDENTIALS` | Azure service principal credentials |

Sensitive credentials are supplied through GitHub Secrets rather than hardcoded in the workflow.

## Key Learning Outcomes

This project provided practical experience with:

- Docker containerisation.
- Automated image builds and publishing.
- Deploying applications to Azure Kubernetes Service.
- Managing Kubernetes Deployments and Services.
- Configuring development and production namespaces.
- Using Azure CLI and kubectl in CI/CD workflows.
- Managing pipeline credentials with GitHub Secrets.
- Organising repository issue and pull request templates.

## Project Status

This project has been completed. The AKS environment used during implementation has since been decommissioned.

The repository retains the application files, Dockerfile, Kubernetes manifests, and GitHub Actions workflow as a record of the implementation.

The deployment workflow describes the original deployment process but is not currently running against an active AKS environment.

## Project Repository

[View Azure Cloud Deployment Automation with AKS](https://github.com/temade91/Azure-Cloud-Deployment-Automation-with-AKS)
