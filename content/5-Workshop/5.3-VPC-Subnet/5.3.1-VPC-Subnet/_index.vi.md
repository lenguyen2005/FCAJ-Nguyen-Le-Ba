---
title : "Tạo VPC và các Subnet"
date : 2026-07-30 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

#### Tạo VPC
1. Mở [Amazon VPC console](https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1#Home:)
2. Trong thanh tìm kiếm, tìm **VPC**, chọn **VPC** và đi tới trang điều khiển VPC.

![VPC](/images/5-Workshop/5.3-VPC-Subnet/Create_VPC.png)

3. Trong **VPC** Console chọn **Create VPC**:
+ Chọn VPC only
+ Đặt tên cho **VPC** là "edushare-vpc"
+ IPv4 CIDR block: Chọn Manual input và nhập `10.0.0.0/16` và click chọn **Create VPC**

![VPC2](/images/5-Workshop/5.3-VPC-Subnet/Create_VPC_2.png)

+ Sau khi đã tạo **VPC**, chọn lại VPC **"edushare-vpc"** và chọn **Actions** để **Edit VPC**
+ Click vào ô tick **"DNS Setting"** để bật **"Enable DNS hostnames"** và **"Enable DNS resolution"** để RDS và ECR có thể phân giải tên miền nội bộ

![VPCDNS](/images/5-Workshop/5.3-VPC-Subnet/Create_VPC_DNS.png)

#### Tạo Subnet
1. Quay lại trang điều khiển VPC, và ở cột bên trái, chọn **Subnets**
+ Bấm chọn **Create subnet**

| Tên Subnet (Name tag) | Availability Zone (AZ) | IPv4 CIDR block | Vai trò |
| :--- | :--- | :--- | :--- |
| `edushare-public-subnet-a` | Chọn AZ đầu tiên (vd: `us-east-1a`) | `10.0.1.0/24` | Chứa NAT Gateway, ALB |
| `edushare-public-subnet-b-dummy` | Chọn AZ thứ hai (vd: `us-east-1b`) | `10.0.2.0/24` | ALB thỏa điều kiện Multi-AZ |
| `edushare-private-app-subnet` | Chọn AZ đầu tiên (`us-east-1a`) | `10.0.10.0/24` | Chứa ECS Fargate (NestJS) |
| `edushare-private-db-subnet` | Chọn AZ đầu tiên (`us-east-1a`) | `10.0.20.0/24` | Chứa RDS PostgreSQL & Redis |

![Subnet](/images/5-Workshop/5.3-VPC-Subnet/Create_Subnet.png)

2. Trong phần tạo Subnet ta tiến hành điền các thông tin:
+ Chọn **VPC** là **"edushare-vpc"**
+ Tên subnet là **"edushare-public-subnet-a"**
+ Chọn **AZ** là **"us-east-1a"**
+ IPv4 CIDR block: `10.0.1.0/24`

3. Làm tương tự với những Subnet còn lại