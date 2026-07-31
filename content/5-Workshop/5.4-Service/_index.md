---
title : "Data Resources & Edge Services"
date : 2026-07-31 
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Overview
In this section, we will provision the foundational data storage services and set up the edge networking layer. These services act as the secure backbone of our architecture, strictly isolated within the VPC or acting as the entry point for incoming traffic before we deploy the actual application code.

#### What we will do:
- **Database & Cache:** Provision a managed **RDS PostgreSQL** database (Single-AZ) in the Private DB Subnet, and an **ElastiCache Redis** node in the Private App Subnet. Both will be protected by the Database Security Group.
- **Storage & Security:** Create an **Amazon S3 Bucket** configured with custom CORS policies to allow the frontend to upload files directly via Presigned URLs. We will also use **AWS Secrets Manager** to securely centralize sensitive credentials (DATABASE_URL, REDIS_HOST, JWT_SECRET, S3_BUCKET_NAME).
- **Load Balancer & SSL:** Request a free SSL/TLS certificate via **AWS Certificate Manager (ACM)** in the `us-east-1` region, then set up an **ALB** and **Target Groups** in our main region to securely route incoming traffic to our future Fargate containers.

![overview](/images/5-Workshop/5.4-Data-Edge/diagram.png)

#### Content
- [1. Create S3](5.4.1-s3_cors/)
- [2. Create Database](5.4.2-rds/)
- [3. Create Elastic Cache](5.4.3-cache/)
- [4. Save Key and Value into Secret Manager](5.4.4-secret-manager/)
- [5. Create Target Group and Load Balancer](5.4.5-target-group-alb/)