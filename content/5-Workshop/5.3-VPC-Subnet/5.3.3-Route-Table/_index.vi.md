---
title : "Tạo Route Table"
date : 2026-07-30 
weight : 3
chapter : false
pre : " <b> 5.3.3 </b> "
---

#### Tạo Route Table
Chúng ta sẽ tạo một Public Route Table cho Public Subnet để định nghĩa định tuyến cho Subnet này để nó đi đến Internet Gateway.

1. Trong Console của **VPC** ở cột bên trái chọn **Route Table**
+ Chọn **"Create Route Table"**
+ Đặt tên bảng Route Table là "edushare-public-rt"
+ Chọn **"Create"**

![Create Route Table](/images/5-Workshop/5.3-VPC-Subnet/Create_RT.png)

2. Quay lại trang Console, chọn vào **"edushare-public-rt"** đã tạo, chọn Tab **Route** ở dưới cuối trang
+ Chọn **Edit Route** và chọn **Add Route**
+ Destination: `0.0.0.0/0`
+ Target chọn Internet Gateway trỏ vào **"edushare-igw"**

![Config Route Table](/images/5-Workshop/5.3-VPC-Subnet/Config_RT.png)

3. Quay lại trang Console, chọn Tab **Subnet Associations** ở dưới cuối trang và chọn **Edit subnet associations**
+ Tick chọn Subnet Public tương ứng là **"edushare-public-subnet-a"**

![Subnet Route Table](/images/5-Workshop/5.3-VPC-Subnet/Subnet_RT.png)

#### Tạo các Route Table khác tương tự
Private App Route Table cho Fargate đi qua NAT Gateway 

- Name: ***"edushare-private-app-rt"** với Target là **"edushare-nat-gw"** và chọn Subnet tương ứng là **"edushare-private-app-subnet"**

Private DB Route Table là mạng cô lập hoàn toàn
- Name: ***"edushare-private-db-rt** thì không cần thêm `0.0.0.0/0` và chọn Subnet tương ứng là **"edushare-private-db-subnet"**




