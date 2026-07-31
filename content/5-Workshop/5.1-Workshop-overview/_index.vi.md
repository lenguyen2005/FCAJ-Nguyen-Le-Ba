---
title : "Introduction"
date : 2026-07-07 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### Tổng quan Workshop
Workshop này cung cấp hướng dẫn từng bước để triển khai hệ thống EduShare lên môi trường AWS, áp dụng kiến trúc Cloud-Native và Serverless:
+ **"VPC"** thiết lập hạ tầng mạng cô lập, sử dụng các Subnet phân tầng (Public/Private) để tạo ranh giới bảo mật nghiêm ngặt.
+ **"ECS Fargate & ECR"** là nền tảng điện toán container serverless dùng để chạy Next.js Frontend và NestJS Backend, kết hợp cùng ECR để lưu trữ Docker image.
+ **"ALB"** phân phối lưu lượng truy cập bằng cơ chế Path-based Routing, cho phép cả Frontend và Backend chạy chung trên một bộ cân bằng tải duy nhất.
+ **"RDS PostgreSQL & ElastiCache Redis"** cung cấp dịch vụ cơ sở dữ liệu quan hệ và bộ nhớ đệm được quản lý hoàn toàn tự động bên trong mạng kín.
+ **"S3 & Secrets Manager"** xử lý việc lưu trữ an toàn các tệp tài liệu (thông qua Presigned URL) và quản lý tập trung các chuỗi bảo mật.
+ **"CloudFront, WAF & Route 53"** (Trên lý thuyết) tạo thành lớp bảo mật biên và CDN giúp phân phối nội dung toàn cầu, chống tấn công web và quản lý tên miền định tuyến.

*Lưu ý: Chúng ta sẽ không thực hành triển khai CloudFront, AWS WAF và Route 53 trong workshop này do tài khoản đang trong quá trình chờ AWS Support xác minh để có quyền tạo tài nguyên CloudFront.*

![overview](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)