---
title : "Networking Infrastructure"
date : 2026-07-30
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Building the Core Network

In this section, you will build the core network foundation for the EduShare system. We will create a **VPC** with isolated subnets, configure internet and NAT gateways, and establish strict security boundaries using **Security Groups** Zero-Trust approach. Finally, you will provision the necessary **IAM Roles** to allow our ECS Fargate containers to securely interact with other AWS services.

#### Content

- [1. Create VPC and Subnetting](5.3.1-vpc-subnet/)
- [2. Create Gateways](5.3.2-gateways/)
- [3. Configure Route Tables](5.3.3-route-tables/)
- [4. Set up Security Groups (Zero-Trust)](5.3.4-security-group/)
- [5. Create IAM Roles for ECS Fargate](5.3.5-iam-roles-cloudwatch/)