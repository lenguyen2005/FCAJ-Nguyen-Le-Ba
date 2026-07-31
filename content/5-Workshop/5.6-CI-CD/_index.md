---
title : "CI/CD Pipeline Automation"
date : 2026-07-30
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Automating the Deployment

In this section, you will build an automated CI/CD (Continuous Integration & Continuous Deployment) pipeline using **GitHub Actions**. This pipeline will automatically build Docker images for both the NestJS Backend and Next.js Frontend, push them to **Amazon ECR**, and seamlessly trigger rolling updates on **Amazon ECS Fargate** without downtime.

#### Content

- [1. Configure IAM User and GitHub Secrets](5.6.1-iam-github-secrets/)
- [2. Create Amazon ECR Repositories](5.6.2-create-ecr/)
- [3. Setup GitHub Actions Workflows](5.6.3-github-actions/)