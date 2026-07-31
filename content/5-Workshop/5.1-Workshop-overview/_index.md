---
title : "Introduction"
date : 2026-07-07 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### Workshop overview
This workshop provides a step-by-step guide to deploying the EduShare system onto an AWS environment, utilizing a Cloud-Native and Serverless architecture:
+ **"VPC"** establishes an isolated network infrastructure, utilizing Tiered Subnets (Public/Private) for strict security boundaries. 
+ **"ECS Fargate & ECR"** are fully serverless containers compute engine hosting the Next.js Frontend and NestJS Backend, paired with ECR for Docker image registry.
+ **"ALB"** distributes incoming traffic using Path-based Routing, allowing both Frontend and Backend to share a single load balancer.
+ **"RDS PostgreSQL & ElastiCache Redis"** provide managed, highly available relational database and caching services within the private network.
+ **"S3 & Secrets Manager"** handle secure object storage for user files (via Presigned URLs) and centralized management of sensitive environment variables.
+ **"CloudFront, WAF & Route 53"** (Theoretical) form the edge security and CDN layer to deliver content globally, protect against web exploits, and manage custom DNS.

*Note: We will not cover the practical deployment of CloudFront, AWS WAF, and Route 53 in this workshop, as the AWS account is currently pending verification from AWS Support to provision CloudFront resources.*

![overview](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)