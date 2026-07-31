---
title: "Worklog Week 4"
date: 2026-07-04
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:
- Finalize the idea and select the project topic
- Learn about Amazon VPC, Subnets, and Security Groups
- Learn about Amazon ECS and the Fargate deployment model

### Tasks to be implemented this week:
| Day | Task | Start Date | Completion Date | Reference Materials |
| --- | ---- | ---------- | --------------- | ------------------- |
| Mon | - Search for and finalize the project topic | 06/29/2026 | 06/29/2026 | |
| Tue | - Identify main features, user analysis, and draft system architecture | 06/30/2026 | 06/30/2026 | |
| Wed | - Research Amazon VPC: Public/Private Subnets, NAT Gateway, Internet Gateway, Security Groups | 07/01/2026 | 07/01/2026 | <https://docs.aws.amazon.com/vpc/> |
| Thu | - Learn about Amazon ECS: Cluster, Service, Task Definition, Fargate Launch Type | 07/02/2026 | 07/02/2026 | <https://docs.aws.amazon.com/ecs/> |
| Fri | - **Practice:** Create a VPC with Public and Private Subnets, configure Security Groups, initialize ECS Cluster Fargate | 07/03/2026 | 07/03/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Achievements in Week 4:

- Finalized the project topic: Building a web platform (document sharing / learning social network) with a **NestJS** backend, deployed on **Amazon ECS Fargate**.

- Defined the problem and project scope:
  - Target users: students, learners, and instructors who need to share learning materials.
  - Goal: Build a scalable, secure web platform using a container-based architecture on AWS.
  - Main features: user registration/login, document upload to S3, search, session management with Redis.

- Understood the role of **Amazon VPC** in isolating and protecting AWS resources:
  - **Public Subnet**: Hosts the ALB and NAT Gateway to receive external traffic.
  - **Private Application Subnet**: Hosts ECS Tasks (NestJS), not directly exposed to the Internet.
  - **Private DB Subnet**: Hosts RDS PostgreSQL, only accessible from the Application Subnet.

- Grasped core concepts of **Amazon ECS Fargate**:
  - Cluster, Service, Task Definition, Container Definition.
  - Fargate Launch Type: no EC2 instance management needed.
  - Task Role and IAM Task Role for granting permissions to containers.

- Practiced creating a VPC from scratch:
  - Created a VPC with an appropriate CIDR block.
  - Created Public and Private Subnets in Availability Zone A.
  - Attached Internet Gateway to the Public Subnet, NAT Gateway to the Private Subnet.
  - Configured tiered Security Groups: ALB SG → ECS SG → DB SG.

- Initialized an ECS Fargate Cluster and ran a test Task.

- Evaluated project feasibility and chose a **Container-based architecture on AWS ECS Fargate**, combined with a CI/CD pipeline via GitHub Actions to automate the build and deploy process.
