---
title : "Tạo ValkeyCache"
date : 2026-07-31
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

ElastiCache là dịch vụ lưu trữ dữ liệu trong bộ nhớ được sử dụng làm bộ nhớ đệm và cơ sở dữ liệu thời gian thực để tăng tốc độ ứng dụng, giảm tải cho hệ thống cơ sở dữ liệu chính, đồng thời hỗ trợ quản lý phiên đăng nhập và hàng đợi. Ở phần này chúng ta sẽ tạo Cache dùng phiên bản mới là Valkey do AWS recommend.

#### Tạo ValkeyCache
1. Ở thanh tìm kiếm tìm **"ElastiCache"** và nhấn chọn, sau đó sẽ hiện ra Console của **"ElastiCache"**

![Create Cache](/images/5-Workshop/5.4-Service/Create_Cache.png)

2. Chọn **Valkey Caches** và chọn **Create cache**
- Trong Cache Setting:
+ Chọn Engine: **Valkey**
+ Deployment opption: Node-based cluster
+ Create method: **Easy Create**

- Trong Configuration:
+ Chọn **Demo**

![Config Cache 1](/images/5-Workshop/5.4-Service/Config_Cache_1.png)

- Ở mục Cluster Info:
+ Name: edushare-redis

![Config Cache 2](/images/5-Workshop/5.4-Service/Config_Cache_2.png)

- Sau đó Click Create để tạo Cache.





