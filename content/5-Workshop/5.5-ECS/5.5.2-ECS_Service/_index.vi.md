---
title : "Khởi tạo Service"
date : 2026-07-31
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---

Bây giờ chúng ta sẽ tiến hành khởi tạo một Service từ Cluster, dựa trên Task Definition để khởi tạo, Pull Image về và chạy. Chúng ta sẽ khởi tạo 2 Service, 1 cái cho Backend và 1 cái cho Frontend. Phần này sẽ hướng dẫn cho Backend còn Frontend làm tương tự.

![Find Cluster](/images/5-Workshop/5.5-ECS/Create_Cluster_2.png)

#### Khởi tạo ECS Service cho Backend
1. Ở thanh tìm kiếm, tìm **ECS** và bấm chọn, sau đó sẽ dẫn đến trang Console của **ECS**
2. Tìm tới Cluster đã tạo trước đó là **edushare-cluster** và bấm chọn
3. Console dẫn đến trang các service của cluster. Chọn **Create service**

![Create Service 1](/images/5-Workshop/5.5-ECS/Create_Service_1.png)

+ Family: Chọn edushare-api-task với phiên bản mới nhất là **: LATEST**
+ Service name: edushare-api-service
+ Launch type: Chọn Fargate.
+ Desired tasks: Nhập 1

![Create Service 2](/images/5-Workshop/5.5-ECS/Create_Service_2.png)

3. Kéo xuống mở rộng tab **Networking**
+ VPC: Chọn edushare-vpc
+ Subnets: Xóa các subnet mặc định, chỉ tick chọn **edushare-private-app-subnet**
+ Cài đặt Public IP: Turn off
+ Security group: Chọn Use an existing security group và xóa cái default đi, tick chọn **edushare-ecs-sg**.
+ Click **Create**

![Create Service 3](/images/5-Workshop/5.5-ECS/Create_Service_3.png)

4. Load balancing
+ Type: Application Load Balancer
+ Load balancer: Chọn **edushare-alb** đã tạo 
+ Container to load balance: Chọn edushare-app:3000
Target group: Chọn Use an existing target group và chọn **edushare-tg**

![Create Service 4](/images/5-Workshop/5.5-ECS/Create_Service_4.png)

![Create Service 5](/images/5-Workshop/5.5-ECS/Create_Service_5.png)

5. Đợi Service được khởi tạo, và hiện ra **"Healthy"**

![Result](/images/5-Workshop/5.5-ECS/Result.png)
6. Tiến hành làm tương tự cho Service Frontend, kết quả cuối cùng thu được sẽ là 2 Service Fargate đang chạy.


