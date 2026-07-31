---
title : "Hạ tầng Mạng (Networking)"
date : 2026-07-30
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Xây dựng Nền tảng Mạng

Trong phần này, bạn sẽ xây dựng nền tảng mạng cốt lõi cho hệ thống EduShare. Chúng ta sẽ tạo một **VPC** với các Subnet được cô lập, cấu hình các Gateways, và thiết lập ranh giới bảo mật nghiêm ngặt bằng **Security Groups** theo mô hình Zero-Trust. Cuối cùng, bạn sẽ tạo các **IAM Roles** cần thiết để cho phép các container ECS Fargate tương tác an toàn với các dịch vụ AWS khác.

![overview](/images/5-Workshop/5.3-Networking/diagram.png)

#### Nội dung

- [1. Tạo VPC và Subnet](5.3.1-vpc-subnet/)
- [2. Tạp Gateways](5.3.2-gateways/)
- [3. Chỉnh sửa Route Tables](5.3.3-route-tables/)
- [4. Tạo Security Groups (Zero-Trust)](5.3.4-security-group/)
- [5. Tạo IAM Roles cho ECS Fargate](5.3.5-iam-roles-cloudwatch/)