---
title : "Thiết lập Security Groups"
date : 2026-07-30 
weight : 4
chapter : false
pre : " <b> 5.3.4 </b> "
---

#### Tạo ALB Security Group
ALB Security Group giúp tạo một Security Group quy định các Inbound Rule để cho phép Load Balancer nằm trong ALB Sercurity Group sau này có thể giao tiếp với Internet.
1. Ở Console của **VPC** chọn ở cột bên trái vào **Security Group**
2. Chọn **Create Security Group**
- Name: edushare-alb-sg
- Inbound rules:
+ Type: HTTP (80) và Source: 0.0.0.0/0 (Anywhere IPv4)
+ Type: HTTPS (443) và Source: 0.0.0.0/0 (Anywhere IPv4)
- Click Create.

![Create ALB Security Group](/images/5-Workshop/5.3-VPC-Subnet/ALB_SG.png)

#### Tạo ECS Security Group
ECS Security Group tạo các Inbound Rule cho phép các Fargate nằm trong ECS Security Group chạy các Image Backend và Frontend sau này có thể giao tiếp với Load Balancer nằm trong **edushare-alb-sg**
- Name: edushare-ecs-sg
- Inbound rules:
+ Type: Custom TCP và Port range: 3000 là Cổng app NestJS của Backend Image chạy trên Fargate
+ Source: Click vào ô tìm kiếm, gõ edushare-alb-sg và chọn Security Group của ALB vừa tạo ở bước trên.
+ Click Create.

![Create ECS Security Group](/images/5-Workshop/5.3-VPC-Subnet/ECS_SG.png)

#### Tạo Database Security Group
Database Security Group thiết lập các Inbound Rule quy định chỉ các dịch vụ nằm trong **edushare-ecs-sg** mới có thể giao tiếp với các dịch vụ nằm trong Security Group này.
- Name: edushare-db-sg
- Inbound rules:
+ Type: PostgreSQL (5432) và Source: chọn **edushare-ecs-sg**
+ Type: Custom TCP, Port range: 6379 là cổng Redis và Source: chọn **edushare-ecs-sg**
+ Click Create.

![Create Database Security Group](/images/5-Workshop/5.3-VPC-Subnet/DB_SG.png)



