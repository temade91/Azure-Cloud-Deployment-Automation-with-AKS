# Netflix Clone – Azure Kubernetes Service Deployment

## Project Overview

This project demonstrates the containerisation and deployment of a Netflix Clone web application using Docker, Kubernetes, Azure Kubernetes Service (AKS), and GitHub Actions.

The application is served using Nginx and packaged into a Docker image. GitHub Actions automates the image build and publication to Docker Hub, followed by deployment to AKS.

The deployment workflow was designed to support two environments:

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

1. A push or pull request targeting `master` or `release` triggers the workflow.
2. GitHub Actions checks out the repository.
3. Docker Buildx builds the application image.
4. The image is pushed to Docker Hub using `latest` and commit-SHA tags.
5. The Kubernetes deployment manifest is updated with the commit-specific image.
6. The workflow authenticates with Azure using GitHub Secrets.
7. AKS credentials are retrieved using Azure CLI.
8. The deployment namespace is selected based on the branch.
9. Kubernetes applies the deployment and service manifests.
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

## Release Branch Validation

A separate GitHub Actions workflow validates pull requests targeting `master`.

It allows pull requests from the `release` branch and fails the check for pull requests originating from other branches.

For this check to prevent merging, its status must also be configured as a required check in GitHub branch protection rules or a ruleset.

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
- Implementing release branch validation.

## Project Status

This is a completed project. The AKS environment used during implementation has since been decommissioned. The repository retains the application files, Dockerfile, Kubernetes manifests, and GitHub Actions workflows as a record of the implementation.

## Project Repository

[View the Netflix Clone project on GitHub](https://github.com/temade91/Netflix--clone-project-)
