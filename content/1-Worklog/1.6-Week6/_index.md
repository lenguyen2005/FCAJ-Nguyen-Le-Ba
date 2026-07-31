---
title: "Worklog Week 6"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

- Deploy the NestJS application to Amazon ECS Fargate
- Set up Application Load Balancer, RDS PostgreSQL, and ElastiCache Redis

### Tasks to be implemented this week:

| Day | Task | Start Date | Completion Date | Reference Materials |
| --- | ---- | ---------- | --------------- | ------------------- |
| Mon | - Create ECS Task Definition for NestJS <br> - Configure IAM Task Role | 07/13/2026 | 07/13/2026 | <https://docs.aws.amazon.com/ecs/> |
| Tue | - Create Application Load Balancer (ALB) <br> - Configure Target Group and Listener | 07/14/2026 | 07/14/2026 | <https://docs.aws.amazon.com/elasticloadbalancing/> |
| Wed | - Initialize RDS PostgreSQL (Single-AZ) in Private DB Subnet <br> - Configure Database Security Group | 07/15/2026 | 07/15/2026 | <https://docs.aws.amazon.com/rds/> |
| Thu | - Set up ElastiCache Redis for session caching <br> - Store credentials securely with AWS Secrets Manager | 07/16/2026 | 07/16/2026 | <https://docs.aws.amazon.com/elasticache/> |
| Fri | - Create ECS Service and perform first deployment <br> - Test end-to-end connectivity via ALB | 07/17/2026 | 07/17/2026 | |

### Achievements in Week 6:

- Successfully deployed the NestJS application to **Amazon ECS Fargate** in the Private Application Subnet.

- Configured the **Application Load Balancer (ALB)**:
  - Listener on port 80/443.
  - Target Group connected to ECS Tasks.
  - Health check path `/health`.

- Initialized **RDS PostgreSQL (Single-AZ)** in the Private DB Subnet:
  - Only allows connections from the ECS Security Group.
  - Configured DB parameters via Parameter Group.

- Integrated **ElastiCache Redis** as a caching layer for sessions and frequent queries.

- Stored sensitive information (DB password, JWT secret) securely using **AWS Secrets Manager**:
  - ECS Tasks automatically fetch secrets at container startup.
  - IAM Task Role configured with `secretsmanager:GetSecretValue` permission.

- Set up **App Auto Scaling** for the ECS Service based on CPU/Memory Utilization.

- Successfully tested the full flow: User → CloudFront → ALB → ECS Task → RDS/Redis.
