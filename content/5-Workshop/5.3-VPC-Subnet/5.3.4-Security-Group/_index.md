---
title : "Set up Security Groups"
date : 2026-07-30 
weight : 4
chapter : false
pre : " <b> 5.3.4 </b> "
---

#### Create ALB Security Group
The ALB Security Group defines the inbound rules that allow the Load Balancer (which will be associated with this group) to receive traffic from the Internet.
1. In the **VPC** Console, from the left navigation pane, select **Security Groups**.
2. Choose **Create security group**.
- Name: edushare-alb-sg
- Inbound rules:
+ Type: HTTP (80) and Source: 0.0.0.0/0 (Anywhere-IPv4)
+ Type: HTTPS (443) and Source: 0.0.0.0/0 (Anywhere-IPv4)
- Click **Create security group**.

![Create ALB Security Group](/images/5-Workshop/5.3-VPC-Subnet/ALB_SG.png)

#### Create ECS Security Group
The ECS Security Group defines the inbound rules that allow the Fargate tasks (running the Backend and Frontend images) to receive traffic exclusively from the Load Balancer located in **edushare-alb-sg**.
- Name: edushare-ecs-sg
- Inbound rules:
+ Type: Custom TCP and Port range: 3000 (The port for the NestJS Backend application running on Fargate)
+ Source: Click on the search box, type `edushare-alb-sg`, and select the ALB Security Group created in the previous step.
+ Click **Create security group**.

![Create ECS Security Group](/images/5-Workshop/5.3-VPC-Subnet/ECS_SG.png)

#### Create Database Security Group
The Database Security Group establishes inbound rules ensuring that only services within **edushare-ecs-sg** can communicate with the database resources located in this Security Group.
- Name: edushare-db-sg
- Inbound rules:
+ Type: PostgreSQL (5432) and Source: select **edushare-ecs-sg**
+ Type: Custom TCP, Port range: 6379 (the port for Redis) and Source: select **edushare-ecs-sg**
+ Click **Create security group**.

![Create Database Security Group](/images/5-Workshop/5.3-VPC-Subnet/DB_SG.png)