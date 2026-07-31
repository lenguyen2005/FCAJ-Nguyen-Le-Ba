---
title: "Worklog Week 8"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

- Finalize the system and build the Workshop documentation

### Tasks to be implemented this week:

| Day | Task | Start Date | Completion Date | Reference Materials |
| --- | ---- | ---------- | --------------- | ------------------- |
| Mon | - Finalize AWS architecture <br> - Draw the overall architecture diagram using eraser.io | 07/27/2026 | 07/27/2026 | |
| Tue | - Design architecture diagram and add illustrative images | 07/28/2026 | 07/28/2026 | |
| Wed | - Write Workshop documentation using Hugo | 07/29/2026 | 07/29/2026 | <https://gohugo.io/> |
| Thu | - Finalize both Vietnamese and English versions <br> - Deploy Workshop to GitHub Pages and perform final checks | 07/30/2026 | 07/30/2026 | |

### Achievements in Week 8:

- Finalized the AWS deployment architecture diagram, covering all components:
  - CI/CD Pipeline: Developer → GitHub → GitHub Actions → Amazon ECR → Amazon ECS.
  - External Access Layer: Route 53 → CloudFront → AWS WAF → Internet Gateway → ALB.
  - Application Layer: ECS Fargate (NestJS) in the Private Application Subnet.
  - Data Layer: RDS PostgreSQL in the Private DB Subnet.
  - Supporting Services: S3, ElastiCache Redis, Secrets Manager, CloudWatch Logs, IAM Task Role.

- Completed the Workshop documentation following the FCAJ structure.

- Built comprehensive content including:
  - Overview.
  - Prerequisite.
  - Architecture.
  - Step-by-step Guide.
  - Testing.
  - Clean-up.

- Added illustrative images, architecture diagrams, and code snippets.

- Completed bilingual versions (Vietnamese and English).

- Successfully deployed the Workshop to GitHub Pages.

- Finalized the report and prepared for the project demo.
