---
title : "Khởi tạo Database"
date : 2026-07-31
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

Sau khi đã có S3, ta cần một Database Postgre để có thể lưu các thông tin nhẹ khác. Ví dụ như tên đăng nhập và mật khẩu của người dùng, hoặc tìm đến Key và Value của File đang được lưu ở trong S3 nếu có Case cần truy vấn trả về, xem trước File đó. Chúng ta sẽ tạo một Database subnet group chứa Database được khởi tạo nằm trong nó.

#### Khởi tạo Database Subnet Group
1. Ở thanh tìm kiếm, tìm **RDS** và bấm chọn, sau đó sẽ dẫn đến trang Console của **RDS**
2. Ở cột bên trái chọn **Subnet group** và chọn **Create subnet group**

![Create Subnet Group](/images/5-Workshop/5.4-Service/DB_Subnet.png)

3. Ở giao diện tạo subnet group:
+ Name: edushare-db-subnet-group
+ VPC: Chọn edushare-vpc
+ Ở phần **Add subnets**: chọn 2 AZ là 1a và 1b.
+ Chọn Subnet: Tick chọn **edushare-private-db-subnet** thỏa điều kiện nằm trong 1 trong 2 AZ mà chúng ta đã chọn.
+ Click **Create** để tạo

![Config Subnet Group](/images/5-Workshop/5.4-Service/DB_Subnet_Config.png)


#### Khởi tạo RDS Instance
1. Tiếp tục ở cột bên trái chọn **Database** và chọn **Create database**
2. Tại giao diện khởi tạo Database:
- Engine Options: chọn Auroa (PostgreSQL Compactible)
- Create method: Full Configuration
- Templates: Dev/Test
- Cluster scalability type: Provisioned để chọn các nguyên mẫu có sẵn

![Create RDS 1](/images/5-Workshop/5.4-Service/Create_DB_1.png)

- Chọn **db.t3.medium** để tiết kiệm chi phí.
- Ở mục **Setting**:
+ DB instance identifier: edushare-db
+ Đặt **Master username** mà bạn muốn
+ Credentials settings: Chọn **Self managed** và nhập mật khẩu cho Master password, lưu ý nhớ lưu lại cả 2 thông tin này.
- Storage: Chọn General Purpose SSD (gp2/gp3) và Allocating storage: 20 GB. Ngoài ra nên tắt tick chọn Enable storage autoscaling để tránh tốn tiền phát sinh

![Create RDS 2](/images/5-Workshop/5.4-Service/Create_DB_2.png)

- Ở mục **Connectivity**:
+ VPC: Chọn edushare-vpc
+ DB subnet group: Chọn edushare-db-subnet-group vừa tạo
+ Public access: Chọn No để cô lập DB khỏi Internet
+ VPC security group (firewall): Chọn **Choose existing** và tick chọn edushare-db-sg.

![Create RDS 3](/images/5-Workshop/5.4-Service/Create_DB_3.png)

3. Click **Create database** và đợi cho đến khi Database được tạo xong.
+ Đợi khoảng 1 đến 5 phút thì phần Database được tạo thành công thì sẽ hiện ra **Available**


