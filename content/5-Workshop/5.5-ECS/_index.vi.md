---
title : "Điều phối và Tính toán"
date : 2026-07-31
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

#### Tổng quan
Trong phần này, chúng ta sẽ triển khai lớp điện toán cốt lõi cho hệ thống EduShare bằng cách sử dụng **Amazon Elastic Container Service (ECS)** kết hợp với **AWS Fargate** (nền tảng chạy container không cần quản lý máy chủ).

Thay vì phải vận hành các máy ảo (EC2) truyền thống, chúng ta sẽ chỉ tập trung vào việc chạy mã nguồn ứng dụng. Chúng ta sẽ quy định cấu hình chạy container thông qua các **Task Definition**, gom nhóm tài nguyên bằng **ECS Cluster**, và cuối cùng là triển khai ứng dụng Backend, Frontend thành các **ECS Service** hoạt động liên tục, được gắn chặt với Application Load Balancer (ALB) để đón nhận luồng truy cập.


#### Nội dung

- [1. Tạo ECS Cluster và Task Definition](5.5.1-cluster-def/)
- [2. Khởi tạo Service](5.5.2-ecs_service/)