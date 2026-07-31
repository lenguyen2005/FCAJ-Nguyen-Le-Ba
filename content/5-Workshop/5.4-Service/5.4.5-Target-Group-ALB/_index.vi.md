---
title : "Tạo Target Group và Load Balancer"
date : 2026-07-31
weight : 5
chapter : false
pre : " <b> 5.4.5 </b> "
---

Chúng ta sẽ tạo các Target Group để đóng vai trò là các đích đến cho traffic. Sau đó tạo thêm 1 Load Balancer đóng vai trò mở rộng dịch vụ, đồng thời ALB cũng cung cấp một DNS sẵn có, giúp chúng ta có thể test sau này.

#### Tạo Target Group
1. Vào trang Console của **EC2** chọn **Target Group** ở cột bên trái
2. Chọn **Create target group**
+ Choose a target type: chọn IP addresses
+ Target group name: edushare-tg
+ Protocol / Port: HTTP
+ Port 3000 là cổng app NestJS

![Create TG1](/images/5-Workshop/5.4-Service/TG_1.png)

+ VPC: Chọn **edushare-vpc**
+ Health check path: Nhập /api/v1/health vì đây là đường dẫn health check có sẵn trong backend.
+ **Click Next** và trang Register targets, không chọn gì vì hiện tại chưa có container Fargate nào chạy và **Click Create target group**.

![Create TG2](/images/5-Workshop/5.4-Service/TG_2.png)

3. Tiến hành làm tương tự cho một Targert Group khác cho front-end tên là **edushare-frontend-tg**
+ Cả Backend và Frontend đều được cấu hình Listen của Port 3000 nên có thể giữa nguyên


#### Tạo ALB
1. Ở cột bên trái, chọn **Load balancers** và chọn **Create Load balancers**
2. Console sẽ dẫn ta đến trang chọn chế độ Load balancers mà ta muốn dùng
+ Chọn **Application Load Balancer**

![Create ALB](/images/5-Workshop/5.4-Service/ALB_1.png)

+ Load balancer name: edushare-alb
+ Scheme: Internet-facing 
+ IP address type: IPv4
+ VPC: Chọn edushare-vpc

![Create ALB2](/images/5-Workshop/5.4-Service/ALB_2.png)

+ Mappings: Tick chọn AZ đầu tiên và chọn `edushare-public-subnet-a`. Tiếp tục tick chọn AZ thứ hai và chọn `edushare-public-subnet-b-dummy`
+ Security groups: Chọn `edushare-alb-sg`.
+ Listeners and routing: Protocol HTTP và Port 80
+ Default action: Trỏ vào Target group `edushare-tg` vừa tạo.
+ Click **Create load balancer**

![Create ALB3](/images/5-Workshop/5.4-Service/ALB_3.png)

3. Sau khi Load Balancer tạo thành công, chọn vào Balancer và tìm **DNS**, hãy lưu DNS đó để sau này ta có thể gọi và test
