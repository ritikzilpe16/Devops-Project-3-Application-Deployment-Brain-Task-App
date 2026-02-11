<div align="center">

# 🧠 Brain Tasks App

![Build Status](https://img.shields.io/badge/Build-Passing-success?style=for-the-badge&logo=appveyor)
![Docker](https://img.shields.io/badge/Docker-Enabled-blue?style=for-the-badge&logo=docker)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Ready-326ce5?style=for-the-badge&logo=kubernetes)
![AWS](https://img.shields.io/badge/AWS-EKS-orange?style=for-the-badge&logo=amazon-aws)

<br />

**A robust, containerized task management application deployed on AWS EKS.**

[Explore Docs](#project-structure) • [View Demo](#) • [Report Bug](#)

</div>

---

## 🚀 Overview

**Brain Tasks App** is a modern web application designed for efficiency and performance. It is containerized using Docker and orchestrated via Kubernetes on AWS EKS. The application is served using Nginx, ensuring high availability and fast static content delivery.

## 📂 Project Structure

This repository is organized to support a seamless CI/CD workflow. Here's a breakdown of the key directories:

### 📦 `dist` & `dist/assets`
The heart of the frontend application.
- **`dist/`**: Contains the production-ready build artifacts.
- **`dist/assets/`**: Stores static resources like images, stylesheets, and compiled JavaScript files.
- **Role**: These files are copied into the Nginx container during the build process to serve the user interface.

### ☸️ `Kubernetes`
Infrastructure as Code (IaC) for container orchestration.
- **`deployment.yaml`**: Defines the `brain-task-app` deployment, managing replicas and container updates.
- **`service.yaml`**: Exposes the application to the network (LoadBalancer/NodePort).
- **Role**: Ensures the application runs reliably on the EKS cluster.

### 🛠️ `scripts`
Automation utilities for deployment and maintenance.
- **`script.sh`**: A helper script to configure `kubectl` for the `demo-eks` cluster and apply the Kubernetes manifests.
- **Role**: Simplifies manual deployments and CI/CD pipeline steps.

---

## ⚙️ Architecture & CI/CD

The project utilizes **AWS CodeBuild** and **CodePipeline** for automated delivery.

1.  **Source**: Code is pushed to the repository.
2.  **Build (`buildspec.yaml`)**:
    -   Logs into **AWS ECR**.
    -   Builds the **Docker** image from the `dockerfile`.
    -   Pushes the image to the ECR repository.
    -   Updates the `Kubernetes/deployment.yaml` with the new image tag.
3.  **Deploy**:
    -   The `appspec.yaml` and scripts facilitate the deployment to the **AWS EKS** cluster.

## 🐳 Docker Configuration

The application runs on a lightweight Nginx Alpine image.

```dockerfile
FROM nginx:alpine
COPY dist/ /usr/share/nginx/html
RUN sed -i 's/listen 80;/listen 3000;/g' /etc/nginx/conf.d/default.conf
EXPOSE 3000
CMD ["nginx", "-g","daemon off;"]
```

- **Base Image**: `nginx:alpine` for minimal footprint.
- **Port**: Exposed on port `3000`.

## 🚀 Getting Started

### Prerequisites
- Docker installed
- AWS CLI configured
- `kubectl` installed

### Local Development
To build and run the container locally:

```bash
# Build the image
docker build -t brain-task-app .

# Run the container
docker run -p 3000:3000 brain-task-app
```

Visit `http://localhost:3000` to view the app.

### Manual Deployment
To deploy to the EKS cluster manually:

```bash
cd scripts
chmod +x script.sh
./script.sh

## 📄 Deployment Documentation

You can view the complete deployment process and screenshots in the following document:

[![Deployment Docs](https://docs.google.com/document/d/1iyii6wPU6JB-XcDP6EVYXSka4oLIg97-/edit?usp=drive_link&ouid=113314245151670066700&rtpof=true&sd=true)

---

<div align="center">

Made with ❤️ by the Ritik Zilpe

</div>