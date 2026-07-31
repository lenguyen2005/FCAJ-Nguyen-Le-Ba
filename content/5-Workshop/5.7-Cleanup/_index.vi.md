---
title : "Dọn dẹp tài nguyên"
date : 2026-07-31
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

#### Dọn dẹp Cluster
1. Truy cập ECS Console
2. Chọn Cluster edushare-cluster
3. Ở tab Services, sau đó bấm *delete*

![Cluster Clean up](/images/5-Workshop/5.7-Cleanup/Cleanup_Cluster.png)

#### Dọn dẹp Load Balancer và Target Groups
1. Ở cột trái, vào Load balancers, tick chọn edushare-alb, chọn Action và Delete load balancer.

![Load balancers Clean up](/images/5-Workshop/5.7-Cleanup/LB_Delete.png)

2. Ở cột trái, vào Target groups, tick chọn edushare-tg và edushare-frontend-tg, chọn Action và Delete.

![Definition Clean up](/images/5-Workshop/5.7-Cleanup/Dereg_Task_Def.png)

#### Xóa Database và Cache
1. Truy cập RDS Console vào Databases. Tick chọn edushare-db, chọn Action và Delete.

![RDS Clean up](/images/5-Workshop/5.7-Cleanup/RDS_Delete.png)

2. ElastiCache Valkey: Truy cập ElastiCache Console -> Valkey caches. Tick chọn edushare-redis, bấm Delete không tạo backup nếu được hỏi.

![Cache Clean up](/images/5-Workshop/5.7-Cleanup/Cache_Delete.png)

#### Xóa NAT Gateway và Elastic IP
Truy cập VPC Console -> NAT Gateways. Chọn edushare-nat-gw, chọn Action -> Delete NAT gateway. Gõ chữ delete để xác nhận.

![NAT Clean up](/images/5-Workshop/5.7-Cleanup/NAT_Delete.png)

#### Xóa S3 và Secrets Manager
1. Truy cập S3 Console.
2. Tick chọn bucket, bấm nút Empty trước, gõ delete để xóa hết file bên trong.
3. Sau khi Empty thành công, chọn lại Bucket đó và bấm nút Delete, gõ tên bucket để xác nhận xóa.
4. Truy cập Secrets Manager, chọn edushare/env-backend-secrets và Actions rồi Delete secret.

![S3 Clean up](/images/5-Workshop/5.7-Cleanup/S3_Delete.png)

![Secret Manager Clean up](/images/5-Workshop/5.7-Cleanup/NAT_Delete.png)

#### Xóa VPC Subnet và Security Group
Truy cập VPC vào chọn vào VPC và xóa

![VPC Clean up](/images/5-Workshop/5.7-Cleanup/VPC_Delete.png)

####Xóa IAM Roles và CloudWatch
1. Truy cập IAM Console và Roles. Tìm và xóa 2 role: edushare-ecs-execution-role và edushare-ecs-task-role.

![IAM Clean up](/images/5-Workshop/5.7-Cleanup/IAM_Delete.png)

2. Truy cập CloudWatch Console và Log groups. Tìm và xóa log group: /aws/ecs/edushare-api và của frontend nếu có.

![CloudWatch Clean up](/images/5-Workshop/5.7-Cleanup/CloudWatch_Delete.png)


