---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# EduShare
## A Learning Resource Sharing Platform Deployed on AWS ECS Fargate

---

### 1. Executive Summary

**EduShare** is a full-stack learning resource sharing platform. The system consists of a frontend built with **Next.js** and a backend built with **NestJS**. The backend is containerized using Docker and deployed on **Amazon ECS Fargate**, while the frontend is deployed separately, providing scalable infrastructure without the need to manage servers.

The deployment architecture leverages Amazon ECS Fargate, Amazon RDS for PostgreSQL, Amazon S3, Amazon CloudFront, AWS WAF, Amazon ElastiCache for Redis, AWS Secrets Manager, and GitHub Actions to build a secure, scalable, and fully automated learning resource sharing platform.

The AWS architecture provides:
- **Security**: AWS WAF, AWS Secrets Manager, and VPC Private Subnets.
- **Performance**: Amazon CloudFront CDN, Amazon ElastiCache for Redis, and ECS Service Auto Scaling.
- **Automation**: CI/CD pipeline using GitHub Actions and Amazon ECR.

---

### 2. Problem Statement

**Current Challenges**

Students and lecturers often face difficulties in sharing and discovering learning resources due to the lack of a centralized, secure, and user-friendly platform. Learning materials are scattered across multiple channels, such as email, chat groups, and personal cloud storage, making them difficult to organize and search efficiently.

**Proposed Solution**

EduShare provides a centralized web platform where users can:
- Register and authenticate using JWT Authentication.
- Upload learning materials (PDF, DOCX, PPTX) to Amazon S3 using Presigned URLs.
- Search for and download learning resources shared by other users.
- Organize learning materials using categories.
- Participate in the Gamification system (EXP, Levels, and Leaderboard).

---

### 3. Solution Architecture

The architecture consists of four main layers:

**CI/CD Layer (CI/CD Pipeline)**
- Developers push source code to a GitHub repository.
- GitHub Actions automatically builds Docker images, pushes them to Amazon ECR, and deploys them to Amazon ECS.

**External Access Layer**
- Users access the application through Amazon Route 53 (custom domain).
- Traffic flows through Amazon CloudFront (CDN) → AWS WAF → Application Load Balancer → Amazon ECS Fargate.
- The frontend requests a Presigned URL from the backend, allowing the browser to upload files directly to Amazon S3, reducing the workload on ECS and improving performance.

**Application Layer (Amazon VPC)**
- Public Subnet: Application Load Balancer and NAT Gateway.
- Private Application Subnet: Amazon ECS Cluster running ECS Services and Fargate Tasks (NestJS) with App Auto Scaling.
- Private Database Subnet: Amazon RDS for PostgreSQL.

**Storage & Supporting Services Layer**
- Amazon S3: Stores learning materials, images, and user avatars.
- Amazon ElastiCache for Redis: Caches sessions and frequently accessed data.
- AWS Secrets Manager: Manages sensitive credentials (database password and JWT secret).
- Amazon CloudWatch Logs: Monitors and collects application logs.
- IAM Task Role: Grants ECS containers secure access to AWS services.

---

### 4. Technical Stack

| Component | Technology |
| ---------- | ---------- |
| Backend Framework | NestJS (TypeScript) |
| Frontend Framework | Next.js (React + TypeScript) |
| Container Runtime | Docker (Multi-stage Build) |
| Container Orchestration | Amazon ECS Fargate |
| Container Registry | Amazon ECR |
| CI/CD | GitHub Actions + IAM OIDC |
| Database | Amazon RDS for PostgreSQL |
| Cache | Amazon ElastiCache for Redis |
| File Storage | Amazon S3 + Presigned URLs |
| CDN & Edge | Amazon CloudFront |
| Security | AWS WAF, AWS Secrets Manager, Amazon VPC |
| DNS | Amazon Route 53 + AWS Certificate Manager (ACM) |
| Monitoring | Amazon CloudWatch Logs & Alarms |

---

### 5. Roadmap & Milestones

| Week | Phase | Activities |
| ---- | ----- | ---------- |
| Week 4 | Design | Finalize the project topic, design the architecture, and study the fundamentals of Amazon VPC and Amazon ECS |
| Week 5 | Backend Development | Develop the NestJS backend, create the Dockerfile, configure Amazon ECR, and implement the GitHub Actions CI/CD pipeline |
| Week 6 | Infrastructure Deployment | Deploy Amazon ECS Fargate, Application Load Balancer, Amazon RDS, Amazon ElastiCache, and AWS Secrets Manager |
| Week 7 | Security & Integration | Configure Amazon CloudFront, AWS WAF, Amazon Route 53, and Amazon S3 Presigned URLs |
| Week 8 | Finalization | Prepare the workshop, documentation, demonstration, and deploy the documentation to GitHub Pages |

---

### 6. Estimated Budget

| Service | Estimated Cost (USD/month) |
| ------- | -------------------------- |
| Amazon ECS Fargate (0.5 vCPU, 1 GB) | ~15 |
| Amazon RDS for PostgreSQL (db.t3.micro) | ~15 |
| Amazon ElastiCache for Redis (cache.t3.micro) | ~12 |
| Amazon CloudFront (10 GB data transfer) | ~1 |
| Amazon S3 (10 GB storage) | ~0.23 |
| Application Load Balancer | ~16 |
| NAT Gateway | ~32 |
| **Total Estimated Cost** | **~91 USD/month** |

---

### 7. Risk Assessment

| Risk | Impact | Mitigation |
| ---- | ------ | ---------- |
| ECS Task failure | Low | ECS Service automatically restarts failed tasks |
| Budget overrun | Low | AWS Budget Alerts and CloudWatch monitoring |
| Credential exposure | Low | AWS Secrets Manager and IAM Task Roles |

---

### 8. Expected Outcomes

- The NestJS backend is successfully containerized with Docker and deployed on Amazon ECS Fargate.
- The CI/CD pipeline automatically builds Docker images, pushes them to Amazon ECR, and deploys them to Amazon ECS whenever changes are merged into the main branch.
- The system supports direct file uploads to Amazon S3 using Presigned URLs.
- The application is distributed through Amazon CloudFront and protected by AWS WAF.
- Data is stored in Amazon RDS for PostgreSQL and cached using Amazon ElastiCache for Redis.
- The system is monitored using Amazon CloudWatch and automatically scales based on workload through ECS Service Auto Scaling.
- Complete workshop documentation is available in both Vietnamese and English.