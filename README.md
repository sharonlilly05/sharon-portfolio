# Portfolio Website CI/CD with Docker Hub (Task 3)

This project demonstrates a simple Docker-based CI/CD workflow using GitHub Actions. Every push to the repository triggers a pipeline that builds a Docker image, logs in to Docker Hub, and pushes the image for deployment.

## Objective

Task 3 focuses on automating the Docker image workflow so that the application can be built and published consistently without manual steps. This improves reliability, reduces human error, and prepares the project for repeatable deployment.

## Prerequisites

Before starting this task, make sure you have:

- Completed Task 2 successfully
- Run the Docker container manually on AWS EC2
- Pushed the project repository to GitHub
- A Docker Hub account

## Project Files

- index.html — portfolio landing page
- style.css — website styling
- Dockerfile — instructions to build the Docker image
- .github/workflows/docker-ci.yml — GitHub Actions workflow for CI/CD

## CI/CD Workflow

The repository uses GitHub Actions to automate the following steps on every push:

1. Build the Docker image
2. Log in to Docker Hub securely using GitHub Secrets
3. Push the image to Docker Hub
4. Keep the image ready for deployment

## Required GitHub Secrets

In your GitHub repository, add these secrets:

- DOCKER_USERNAME — your Docker Hub username
- DOCKER_PASSWORD — your Docker Hub password or access token

These secrets must be used instead of hardcoded credentials.

## Workflow File

Create the workflow file at .github/workflows/docker-ci.yml with a structure like this:

```yaml
name: Docker CI

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ secrets.DOCKER_USERNAME }}/portfolio-website:latest
```

## Local Docker Commands

Build and test locally before pushing:

```bash
docker build -t portfolio-website .
docker run -d -p 8080:80 portfolio-website
```

Verify the running container:

```bash
docker ps
```

## Verification

After pushing to GitHub:

1. Open the GitHub Actions tab
2. Confirm that the workflow run succeeds
3. Pull and run the image from Docker Hub to verify it works:

```bash
docker pull <your-dockerhub-username>/portfolio-website:latest
docker run -d -p 8080:80 <your-dockerhub-username>/portfolio-website:latest
```

## Deliverables

Submit the following:

- GitHub repository containing the CI workflow
- Screenshot of a successful GitHub Actions run
- Link to the Docker Hub image
- Updated README explaining the CI process

## Notes

This README documents the automation process for Task 3 and should be updated if the workflow or deployment steps change.
