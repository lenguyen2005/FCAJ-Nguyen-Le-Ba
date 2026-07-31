---
title : "Tạo IAM Roles và CloudWatch"
date : 2026-07-30 
weight : 5
chapter : false
pre : " <b> 5.3.5 </b> "
---

Ở phần này, chúng ta sẽ tạo IAM Roles cho ECS Fargate để nó có quyền cho ECS kéo Image và ghi Log, cũng như cho code NestJS thao tác S3

Ở thanh tìm kiếm, tìm **IAM** và chọn, sau đó ở cột bên trái chọn **Role**

![Create IAM](/images/5-Workshop/5.3-VPC-Subnet/IAM_Create.png)

#### Tạo Task Execution Role
Ta tạo quyền cho ECS kéo Image và Ghi Log
1. Chọn **Create Role**
2. Ở Step 1:
+ Trusted entity type: Chọn AWS service
+ Use case: Chọn Elastic Container Service và chọn tiếp Elastic Container Service Task

3. Ở Step 2:
+ Ở ô tìm kiếm permissions, gõ và tick chọn policy có tên: AmazonECSTaskExecutionRolePolicy
+ Tìm và tick chọn thêm policy: SecretsManagerReadWrite để Fargate đọc được biến môi trường từ Secrets Manager

4. Ở Step 3:
+ Role name: edushare-ecs-execution-role

![Create IAM Permission](/images/5-Workshop/5.3-VPC-Subnet/IAM_Per.png)

#### Tạo Task Role
Tạo quyền cho code NestJS thao tác S3
1. Chọn **Create Role**
2. Ở Step 1:
+ Trusted entity type: Chọn AWS service
+ Use case: Chọn Elastic Container Service và chọn tiếp Elastic Container Service Task

3. Ở Step 2:
+ Ở ô tìm kiếm permissions, gõ và tick chọn policy có tên AmazonS3FullAccess để app NestJS tạo được Presigned URL cho user upload tài liệu lên S3

4. Ở Step 3:
+ Role name: edushare-ecs-task-role

#### Tạo CloudWatch
1. Chuyển qua dịch vụ **CloudWatch** trên thanh tìm kiếm
2. Ở cột bên trái vào **Log Managment** và chọn **Create log group**
+ Log group name: /aws/ecs/edushare-api
+ Retention setting: Chọn 7 days để tránh log lưu quá lâu tốn chi phí

![Create CloudWatch](/images/5-Workshop/5.3-VPC-Subnet/CW.png)



