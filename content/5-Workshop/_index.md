---
title: "Workshop"
date: 2026-07-31
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
# EduShare for Sharing Document using AWS Services

#### Overview

**EduShare** is a modern, highly scalable platform designed to foster academic collaboration and seamless knowledge sharing. Moving beyond a traditional document repository, EduShare cultivates a vibrant learning community by integrating real-time discussions, Q&A, and an engaging Gamification system (experience points, leveling, and real-time leaderboards) to motivate user contributions.

1. Key Technical Highlights: Built with performance and scalability at its core, EduShare adopts a strict Clean Architecture (NestJS) on the backend and a Feature-based Modular (Next.js) approach on the frontend. The system is fully powered by a robust AWS Cloud Infrastructure, featuring:

2. Lightning-Fast Content Delivery: Direct-to-cloud file uploads via S3 Presigned URLs and global caching powered by Amazon CloudFront.

3. High Performance & Scalability: Real-time data caching and leaderboards using Amazon ElastiCache (Redis), with the core API deployed on serverless containers via Amazon ECS (Fargate) for effortless auto-scaling.

4. Enterprise-Grade Security: Centralized secret management with AWS Secrets Manager and strict IAM policies.

EduShare is not just an educational tool; it is a testament to applying modern software design principles and cloud infrastructure to solve real-world scalability challenges.

#### Content

1. [Workshop overview](5.1-Workshop-overview)
2. [Prerequiste](5.2-Prerequiste/)
3. [Infrastructure](5.3-VPC-Subnet/)
4. [Service](5.4-Service/)
5. [VPC Endpoint Policies (Bonus)](5.5-Policy/)
6. [Clean up](5.6-Cleanup/)