---
title: "Worklog Week 5"
date: 2026-7-11
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

- Build the NestJS application and containerize it with Docker
- Set up Amazon ECR and CI/CD pipeline with GitHub Actions

### Tasks to be implemented this week:
| Day | Task | Start Date | Completion Date | Reference Materials |
| --- | ---- | ---------- | --------------- | ------------------- |
| Mon | - Initialize NestJS project <br> - Build basic module structure (Auth, User, Document) | 07/06/2026 | 07/06/2026 | <https://docs.nestjs.com/> |
| Tue | - Write multi-stage Dockerfile for NestJS <br> - Test local image build | 07/07/2026 | 07/07/2026 | |
| Wed | - Create Amazon ECR Repository <br> - Push Docker image to ECR for the first time | 07/08/2026 | 07/08/2026 | <https://docs.aws.amazon.com/ecr/> |
| Thu-Fri | - Set up GitHub Actions Workflow: build → push ECR → deploy ECS <br> - Configure IAM Role for GitHub Actions (OIDC) | 07/09/2026 | 07/10/2026 | <https://docs.github.com/en/actions> |

### Achievements in Week 5:

- Successfully initialized a **NestJS** project with a clean module structure following Domain-Driven Design.

- Understood how to organize code by modules in NestJS:
  - Auth Module: registration, login, JWT.
  - User Module: user profile management.
  - Document Module: upload, search, and manage documents.

- Successfully built a **multi-stage Dockerfile**:
  - Stage 1 (builder): install dependencies and compile TypeScript.
  - Stage 2 (production): copy dist and node_modules for a lean image.

- Understood the workflow with **Amazon ECR**:
  - Create a Private Repository.
  - Authenticate Docker with ECR using AWS CLI.
  - Tag and push image to ECR.

- Set up a complete **GitHub Actions CI/CD Pipeline**:
  - Triggered on push to the `main` branch.
  - Build Docker image.
  - Push image to Amazon ECR.
  - Update ECS Service with the new image tag.

- Configured **IAM OIDC** for GitHub Actions to authenticate securely without storing AWS credentials in secrets.
