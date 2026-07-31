---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# EduShare
## A Document Sharing Platform Deployed on AWS ECS Fargate

---

### 1. Executive Summary

**EduShare** is a web platform that enables students, learners, and instructors to securely and efficiently share learning materials. The system backend is built with **NestJS**, containerized as a Docker image, and deployed on **Amazon ECS Fargate**, ensuring flexible scalability without the need to manage server infrastructure.

The architecture leverages AWS services to ensure:
- **Security**: AWS WAF, Secrets Manager, VPC Private Subnets.
- **Performance**: CloudFront CDN, ElastiCache Redis, App Auto Scaling.
- **Automation**: CI/CD pipeline with GitHub Actions and Amazon ECR.

---

### 2. Problem Statement

**Current Problem**

Students and instructors currently struggle to share and find learning materials due to the lack of a centralized, secure, and easy-to-use platform. Documents are scattered across multiple channels (email, chat groups, personal cloud storage), making retrieval and management time-consuming.

**Proposed Solution**

EduShare provides a centralized web platform where users can:
- Register and log in using JWT Authentication.
- Upload documents (PDF, DOCX, PPTX) to Amazon S3 via Presigned URLs.
- Search and download documents shared by others.
- Manage their personal profile and avatar.

---

### 3. Solution Architecture

The architecture consists of 4 main layers:

**CI/CD Pipeline Layer**
- Developer pushes code to the GitHub Repository.
- GitHub Actions automatically builds the Docker image, pushes it to Amazon ECR, and deploys it to Amazon ECS.

**External Access Layer**
- Users access the platform via Amazon Route 53 (custom domain).
- Traffic flows through Amazon CloudFront (CDN) → AWS WAF (protection) → Internet Gateway → Application Load Balancer.
- Large file uploads go directly to S3 via Presigned URLs.

**Application Layer (Amazon VPC)**
- Public Subnet: Application Load Balancer, NAT Gateway.
- Private Application Subnet: Amazon ECS Cluster running Fargate Tasks (NestJS), with App Auto Scaling.
- Private DB Subnet: Amazon RDS PostgreSQL (Single-AZ).

**Storage & Supporting Services**
- Amazon S3: Storage for documents, images, and avatars.
- ElastiCache Redis: Cache for sessions and frequently queried data.
- AWS Secrets Manager: Manages credentials (DB password, JWT secret).
- Amazon CloudWatch Logs: System monitoring and logging.
- IAM Task Role: Grants ECS containers permissions to access AWS services.

---

### 4. Technical Implementation

| Component | Technology |
| --------- | ---------- |
| Backend Framework | NestJS (TypeScript) |
| Container Runtime | Docker (multi-stage build) |
| Container Orchestration | Amazon ECS Fargate |
| Container Registry | Amazon ECR |
| CI/CD | GitHub Actions + IAM OIDC |
| Database | Amazon RDS PostgreSQL (Single-AZ) |
| Cache | ElastiCache Redis |
| File Storage | Amazon S3 + Presigned URL |
| CDN & Edge | Amazon CloudFront |
| Security | AWS WAF, Secrets Manager, VPC |
| DNS | Amazon Route 53 + ACM SSL |
| Monitoring | Amazon CloudWatch Logs & Alarms |

---

### 5. Timeline & Milestones

| Week | Phase | Content |
| ---- | ----- | ------- |
| Week 4 | Design | Finalize project topic, design architecture, learn VPC and ECS basics |
| Week 5 | Backend Development | Build NestJS, Dockerfile, ECR, GitHub Actions CI/CD |
| Week 6 | Infrastructure Deployment | ECS Fargate, ALB, RDS, Redis, Secrets Manager |
| Week 7 | Security & Integration | CloudFront, WAF, Route 53, S3 Presigned URL |
| Week 8 | Finalization | Workshop documentation, demo, deploy to GitHub Pages |

---

### 6. Budget Estimation

| Service | Estimated Cost (USD/month) |
| ------- | ------------------------- |
| Amazon ECS Fargate (0.5 vCPU, 1 GB) | ~$15 |
| Amazon RDS PostgreSQL db.t3.micro | ~$15 |
| ElastiCache Redis cache.t3.micro | ~$12 |
| Amazon CloudFront (10 GB transfer) | ~$1 |
| Amazon S3 (10 GB storage) | ~$0.23 |
| Application Load Balancer | ~$16 |
| NAT Gateway | ~$32 |
| **Total Estimated** | **~$91 USD/month** |

---

### 7. Risk Assessment

| Risk | Severity | Mitigation |
| ---- | -------- | ---------- |
| RDS Single-AZ failure | Medium | Daily automatic snapshots; can upgrade to Multi-AZ |
| ECS Task crash | Low | ECS Service automatically restarts the Task |
| Budget overrun | Low | CloudWatch Budget Alarm |
| Credentials security | Low | AWS Secrets Manager + IAM Task Role |

---

### 8. Expected Outcomes

- A stable NestJS backend running on Amazon ECS Fargate with auto scaling.
- A fully automated CI/CD pipeline from GitHub to ECS in under 5 minutes.
- Users can successfully register, log in, upload, and download documents.
- A complete bilingual Workshop documentation in Vietnamese and English.