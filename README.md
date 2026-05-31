# 🌐 Multi-Cloud Automated Development and Deployment

> A production-ready React application with fully automated CI/CD infrastructure on AWS, integrating GCP APIs for extended functionality.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-gtc--traveling.duckdns.org-blue?style=for-the-badge&logo=amazonaws)](http://gtc-traveling.duckdns.org/)
[![AWS](https://img.shields.io/badge/AWS-EC2%20%7C%20CodePipeline%20%7C%20S3-orange?style=for-the-badge&logo=amazon-aws)](https://aws.amazon.com/)
[![GCP](https://img.shields.io/badge/GCP-API%20Integration-4285F4?style=for-the-badge&logo=google-cloud)](https://cloud.google.com/)
[![React](https://img.shields.io/badge/React-Vite-61DAFB?style=for-the-badge&logo=react)](https://vitejs.dev/)

---

## 📌 Overview

This project demonstrates a production-grade cloud deployment pipeline built using AWS-native services. A React (Vite) application is continuously built, tested, and deployed to EC2 instances via CodePipeline — all triggered automatically on GitHub commits. GCP APIs are integrated for extended cloud functionality, showcasing true multi-cloud capability.

---

## 🏗️ Architecture

```
GitHub Push
    │
    ▼
AWS CodePipeline
    ├── Source Stage   →  GitHub Repository
    ├── Build Stage    →  AWS CodeBuild  (npm install, vite build)
    │                       └── Artifacts stored in S3
    └── Deploy Stage   →  AWS CodeDeploy → EC2 Instance
                                └── Nginx (Reverse Proxy)
```

> Architecture diagram available in the repository (`Multi cloud arch.png`).

---

## ✨ Features

- 🚀 **Automated CI/CD** — Full pipeline with CodePipeline, CodeBuild, and CodeDeploy; zero manual deployment steps
- ⚡ **Vite-powered React** — Optimized build output with fast Hot Module Replacement during development
- ☁️ **Multi-Cloud** — AWS infrastructure + GCP API integrations in a single production app
- 🔒 **IAM Security Best Practices** — Least-privilege roles for all pipeline and EC2 interactions
- 📊 **CloudWatch Monitoring** — Integrated metrics, logs, and alerts across all services
- 🔁 **Rollback Capabilities** — Automated health checks with deployment rollback on failure
- 📱 **Mobile-First Design** — Fully responsive UI following mobile-first design principles
- 🌐 **Nginx Reverse Proxy** — Configured for production-grade request handling and SSL termination

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React, Vite |
| CI/CD | AWS CodePipeline, CodeBuild, CodeDeploy |
| Compute | AWS EC2 |
| Artifact Storage | AWS S3 |
| Infrastructure as Code | AWS CloudFormation |
| Monitoring | AWS CloudWatch |
| Web Server | Nginx |
| Security | AWS IAM |
| External APIs | GCP APIs |

---

## 🚀 Getting Started

### Prerequisites

- Node.js ≥ 18
- AWS CLI configured with appropriate permissions
- An EC2 instance with the CodeDeploy agent installed
- S3 bucket for build artifacts

### Local Development

```bash
# Clone the repository
git clone https://github.com/PranavGediya/<repo-name>.git
cd <repo-name>

# Install dependencies
npm install

# Start the development server
npm run dev
```

### Deploy via Pipeline

Push to the `main` branch — the pipeline triggers automatically.

```bash
git add .
git commit -m "feat: your changes"
git push origin main
```

CodePipeline picks up the commit, builds via CodeBuild, stores artifacts in S3, and deploys to EC2 via CodeDeploy.

---

## ⚙️ AWS Infrastructure Setup

### CodePipeline

1. Create a new pipeline in AWS CodePipeline.
2. Connect your GitHub repository as the source.
3. Add a CodeBuild build stage using the `buildspec.yml` at the repo root.
4. Add a CodeDeploy deployment stage targeting your EC2 instance group.

### `buildspec.yml` (example)

```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      nodejs: 18
    commands:
      - npm install
  build:
    commands:
      - npm run build

artifacts:
  files:
    - '**/*'
  base-directory: dist
```

### Nginx Configuration

```nginx
server {
    listen 80;
    server_name your-domain.duckdns.org;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

---

## 🔐 IAM Roles Required

| Role | Purpose |
|---|---|
| `CodePipelineRole` | Access to S3, CodeBuild, CodeDeploy |
| `CodeBuildRole` | S3 artifact read/write, CloudWatch Logs |
| `CodeDeployRole` | EC2 instance registration, deployment |
| `EC2InstanceProfile` | S3 artifact read, CloudWatch agent |

---

## 📊 Monitoring

CloudWatch is configured to capture:
- EC2 CPU, memory, and disk metrics
- CodeBuild build logs
- CodeDeploy deployment events and health check results
- Application-level logs from Nginx

---

## 📁 Project Structure

```
├── src/                    # React application source
│   ├── components/
│   ├── pages/
│   └── main.jsx
├── public/
├── dist/                   # Built output (generated)
├── appspec.yml             # CodeDeploy deployment spec
├── buildspec.yml           # CodeBuild build spec
├── scripts/                # CodeDeploy lifecycle hooks
│   ├── before_install.sh
│   ├── after_install.sh
│   └── start_server.sh
├── Multi cloud arch.png    # Architecture diagram
└── README.md
```

---

## 🌍 Live Demo

🔗 [http://gtc-traveling.duckdns.org/](http://gtc-traveling.duckdns.org/)

---

## 👤 Author

**Pranav Gediya**
Multi-Cloud Developer | AWS & OCI Certified | GCP | DevOps

[![Portfolio](https://img.shields.io/badge/Portfolio-pranavgediya.github.io-informational?style=flat&logo=github)](https://pranavgediya.github.io/Portfolio/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-pranav--gediya-blue?style=flat&logo=linkedin)](https://linkedin.com/in/pranav-gediya-47bb57249)
[![GitHub](https://img.shields.io/badge/GitHub-PranavGediya-black?style=flat&logo=github)](https://github.com/PranavGediya)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
