# Changelog

### 2024-09-01

## Added

- **Dockerfile**: Added a Dockerfile to the root directory to build a Docker image for the application. This Dockerfile includes setup for the application environment and dependencies.

- **GitHub Actions Workflow**: Added a GitHub Actions workflow configuration in .github/workflows for CI/CD. The workflow automates the building and pushing of Docker images to GitHub Container Registry (GHCR).

    - The ***.github/workflow*** folder has a *README* file with short description of the CI flow.
    - How to Pull and build container image from GHCR
    - Access and Trigger GitHub Actions workflow
- An Image with the latest succesful workflow

## Modified
- README.md (Root Folder): Updated the root README.md to include instructions for building and running the Docker container. The new instructions cover:

    - Building the Docker image.
    - Running the Docker container locally.
    - Accessing application services.
