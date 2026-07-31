---
title : "Tạo ECS Cluster và Task Definition"
date : 2025-07-30
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---

Ở phấn này chúng ta sẽ tiến hành tạo ECS Cluster và Task Definition. Task Definition là bản thiết kế chi tiết hay file cấu hình quy định cách một hoặc nhiều container sẽ chạy, bao gồm Docker image, CPU, RAM và port. ECS Cluster là một nhóm logic các tài nguyên hoặc máy chủ như EC2 hoặc Fargate dùng để triển khai và chạy các Task đó.

#### Tạo ECS Cluster
1. Vào thanh tìm kiếm tìm **Elastic Container Service** và chọn sẽ dẫn đến Console đó
2. Chọn **Create cluster**
+ Cluster name: edushare-cluster
+ Infrastructure: AWS sẽ mặc định tick chọn AWS Fargate serverless
+ Click **Create**

![Create Cluster](/images/5-Workshop/5.5-ECS/Create_Cluster_1.png)

#### Định nghĩa các Task Definition
Ta sẽ định nghĩa các Task Definition cho Backend và Frontend. Ở đây ta sẽ làm Backend trước còn Frontend làm tương tự.
1. Ở cột bên trái, chọn **Task Definition**
2. Chọn **Create task definition** mới
+ Task definition family: edushare-api-task
+ Launch type: Chọn AWS Fargate
+ OS, Architecture: Linux/X86_64 

![Create Task Manager 1](/images/5-Workshop/5.5-ECS/Task_Def_1.png)

+ CPU: .5 vCPU
+ Memory: 1 GB
+ Task role: Chọn **edushare-ecs-task-role** là quyền cho app NestJS thao tác S3
+ Task execution role: Chọn **edushare-ecs-execution-role** là quyền cho AWS pull image và fetch secret

![Find ECR](/images/5-Workshop/5.5-ECS/ECR.png)

3. Mở tab mới của AWS, ở thanh tìm kiếm tìm **ECR** để đi tới Registry chứa các Image của Backend và Frontend có sẵn
+ Copy URI của Image Backend

4. Quay lại Task Definition
- Ở Container - 1:
+ Name: edushare-app
+ Image URI: Dán chuỗi ECR Image URI vào đây
+ Container port: 3000
+ Protocol: TCP

![Create Task Manager 2](/images/5-Workshop/5.5-ECS/Task_Def_2.png)

- Environment variables
+ Chọn **"Add Environment variables"**
+ Ta tiến hành nhập lại các trường giống như đã lưu trong Secret Manager trước đó
+ Chọn **ValueFrom**
+ Ở phần Value, quay lại Secret Manager đã tạo và copy phần ARN về
+ Ví dụ Key là **DATABASE_URL** thì Value sẽ là `<Secret Manager ARN>:DATABASE_URL::`

![Create Task Manager 3](/images/5-Workshop/5.5-ECS/Task_Def_3.png)

- Log collection: Bật tick Use log collection 
+ Log group: Nhập /aws/ecs/edushare-api đã tạo từ trước.

5. Chọn **Create**
6. Làm tương tự tạo một Task Definition cho Frontend tên là **edushare-frontend-task**
+ Lưu ý, nhớ chọn URI Image của Frontend ở trang ECR
