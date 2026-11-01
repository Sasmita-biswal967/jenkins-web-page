<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/6b38a99f-5c20-4536-9eff-8ac438ff037c" />
# GitHub × Jenkins — Automated CI/CD Pipeline Dashboard

A web-based dashboard for my automated CI/CD pipeline using **GitHub, Jenkins, Docker, Docker Hub, and AWS EC2**. The project demonstrates how a code change pushed to GitHub can automatically trigger a Jenkins pipeline, build a Docker image, publish it to Docker Hub, and deploy the updated website on an AWS EC2 instance.

## Overview

The dashboard provides a simple interface to access the GitHub repository and Jenkins server. It documents the pipeline workflow and the setup checklist required to configure automated builds and deployments.

The actual pipeline execution and build status are managed by Jenkins and the configured GitHub webhook.

## Tech Stack

- **Frontend:** HTML, CSS, JavaScript
- **Version Control:** Git and GitHub
- **CI/CD:** Jenkins
- **Containerization:** Docker
- **Container Registry:** Docker Hub
- **Cloud Platform:** AWS EC2
- **Web Server:** Nginx (Docker container)

## CI/CD Workflow

```text
Developer
    |
    | Push code changes
    v
GitHub Repository
    |
    | GitHub Webhook
    v
Jenkins Pipeline
    |
    | 1. Checkout source code
    | 2. Build Docker image
    | 3. Push image to Docker Hub
    | 4. Deploy updated container
    v
Docker Hub
    |
    | Pull published image
    v
AWS EC2 Instance
    |
    v
Live Website
```

### Pipeline Stages

1. **GitHub — Source Code Management:** The project source code and pipeline configuration are maintained in GitHub.
2. **Webhook — Automated Trigger:** A GitHub webhook notifies Jenkins when code is pushed to the configured branch.
3. **Build — Docker Image:** Jenkins checks out the repository and builds a Docker image using the project's Dockerfile.
4. **Publish — Docker Hub:** The pipeline authenticates using Jenkins-managed credentials and pushes the image to Docker Hub.
5. **Deploy — AWS EC2:** The EC2 instance pulls the published image and runs the updated website container.

## Dashboard Features

- GitHub repository access through the dashboard.
- Jenkins server access through the dashboard.
- Visual representation of the CI/CD pipeline workflow.
- Setup checklist for configuring the Jenkins pipeline and GitHub webhook.
- Links to the GitHub repository and Jenkins server.

> The dashboard is an interface for accessing and understanding the pipeline. Actual build execution and automation are handled by Jenkins and its webhook configuration.

## Project Configuration

| Component | Configuration |
|---|---|
| GitHub Repository | [jenkins-web-page](https://github.com/Sasmita-biswal967/jenkins-web-page) |
| GitHub Branch | `main` |
| Jenkins | Hosted on AWS EC2 |
| Docker Hub Image | `sasmita205/github-jenkins-web` |
| Container Name | `github-jenkins-web` |
| Website Port | `80` |
| Container Web Server | Nginx |

## Setup and Configuration

### Prerequisites

- An AWS EC2 instance running Ubuntu.
- Jenkins installed and configured.
- Docker installed and running on EC2.
- A GitHub repository containing the website source code and Dockerfile.
- A Docker Hub account and repository.
- Jenkins credentials configured for Docker Hub authentication.
- Appropriate EC2 security group rules.

### 1. Clone the Repository

```bash
git clone https://github.com/Sasmita-biswal967/jenkins-web-page.git
cd jenkins-web-page
```

### 2. Build the Docker Image

The Dockerfile uses Nginx to serve the static website.

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

Build the image:

```bash
docker build -t sasmita205/github-jenkins-web:latest .
```

### 3. Configure the Jenkins Pipeline

1. Create a new **Pipeline** job in Jenkins.
2. Set the definition to **Pipeline script from SCM**.
3. Select Git as the source control system.
4. Add the GitHub repository URL.
5. Set the branch to `*/main`.
6. Set the script path to `Jenkinsfile`.
7. Configure the Docker Hub credential in Jenkins.
8. Save the job and run the pipeline.

### 4. Configure the GitHub Webhook

1. Open the GitHub repository settings.
2. Navigate to **Webhooks** and add a webhook.
3. Configure the payload URL to point to the Jenkins webhook endpoint.
4. Set the content type to `application/json`.
5. Select the push event.
6. Save the webhook and verify that a repository push triggers Jenkins.

The Jenkins endpoint must be reachable by GitHub for webhook delivery to work.

### 5. Verify Deployment

After a successful pipeline execution, verify the running container on EC2:

```bash
sudo docker ps
curl -I http://localhost
```

Open the EC2 public IPv4 address in a browser to access the deployed website:

```text
http://<EC2-PUBLIC-IP>
```

Ensure the EC2 security group permits inbound HTTP traffic on port `80`.

## Repository Structure

```text
jenkins-web-page/
├── index.html
├── Dockerfile
├── Jenkinsfile
└── README.md
```

## Learning Outcomes

- Implementing a GitHub-based CI/CD workflow.
- Automating builds with Jenkins pipelines.
- Building and publishing Docker images.
- Deploying a containerized website on AWS EC2.
- Configuring GitHub webhooks for automated pipeline execution.
- Managing deployment credentials through Jenkins.

## Author

**Sasmita Biswal**

GitHub: [Sasmita-biswal967](https://github.com/Sasmita-biswal967)

---

*Built as a hands-on project to learn and demonstrate automated CI/CD deployment using GitHub, Jenkins, Docker, Docker Hub, and AWS EC2.*

