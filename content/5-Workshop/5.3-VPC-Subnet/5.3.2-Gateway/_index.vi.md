---
title : "Tạo Internet Gateway và NAT Gateway"
date : 2026-07-30 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Tạo Internet Gateway
1. Trong Console của **VPC** ở cột bên trái chọn **Internet Gateway**
+ Chọn **Create internet gateway**
+ Điền tên của Gateway là "edushare-igw"
+ Chọn **Create internet gateway** để tạo Internet Gateway

![Create IG](/images/5-Workshop/5.3-VPC-Subnet/Create_IG.png)

2. Quay lại Console của **VPC** ở **Internet Gateway**, sau đó chọn **"edushare-igw"** và bấm **Action**
+ Chọn **Attach VPC** và chọn vào VPC đã tạo là **"edushare-vpc"**

![Attach IG](/images/5-Workshop/5.3-VPC-Subnet/Attach_VPC_IG.png)

#### Tạo NAT Gateway

1. Trong Console của **VPC** ở cột bên trái chọn **NAT Gateway**
2. Chọn **Create NAT Gateway**
+ **Name tag** là edushare-nat-gw
+ **Subnet** chọn **"edushare-public-subnet-a"**
+ **Connectivity type** là "Public".
+ **Elastic IP allocation ID** click vào nút **Allocate Elastic IP** để AWS sẽ tự cấp 1 IP tĩnh gắn vào NAT này
+ **Click Create NAT gateway**

![Create NAT](/images/5-Workshop/5.3-VPC-Subnet/Create_NAT.png)