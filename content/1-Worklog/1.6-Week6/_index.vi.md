---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

- Triển khai ứng dụng NestJS lên Amazon ECS Fargate
- Thiết lập Application Load Balancer, RDS PostgreSQL và ElastiCache Redis

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tạo ECS Task Definition cho NestJS <br> - Cấu hình IAM Task Role | 13/07/2026 | 13/07/2026 | <https://docs.aws.amazon.com/ecs/> |
| 3 | - Tạo Application Load Balancer (ALB) <br> - Cấu hình Target Group và Listener | 14/07/2026 | 14/07/2026 | <https://docs.aws.amazon.com/elasticloadbalancing/> |
| 4 | - Khởi tạo RDS PostgreSQL (Single-AZ) trong Private DB Subnet <br> - Cấu hình Database Security Group | 15/07/2026 | 15/07/2026 | <https://docs.aws.amazon.com/rds/> |
| 5 | - Thiết lập ElastiCache Redis cho session cache <br> - Lưu trữ credentials bằng AWS Secrets Manager | 16/07/2026 | 16/07/2026 | <https://docs.aws.amazon.com/elasticache/> |
| 6 | - Tạo ECS Service và deploy lần đầu <br> - Kiểm thử kết nối end-to-end qua ALB | 17/07/2026 | 17/07/2026 | |

### Kết quả đạt được tuần 6:

- Triển khai thành công ứng dụng NestJS lên **Amazon ECS Fargate** trong Private Application Subnet.

- Cấu hình **Application Load Balancer (ALB)**:
  - Listener port 80/443.
  - Target Group kết nối đến ECS Tasks.
  - Health check path `/health`.

- Khởi tạo **RDS PostgreSQL (Single-AZ)** trong Private DB Subnet:
  - Chỉ cho phép kết nối từ ECS Security Group.
  - Cấu hình thông số DB qua Parameter Group.

- Tích hợp **ElastiCache Redis** làm lớp cache cho session và tăng hiệu năng truy vấn.

- Lưu trữ an toàn các thông tin nhạy cảm (DB password, JWT secret) bằng **AWS Secrets Manager**:
  - ECS Task tự động fetch secrets tại thời điểm khởi động container.
  - IAM Task Role được cấu hình quyền `secretsmanager:GetSecretValue`.

- Thiết lập **App Auto Scaling** cho ECS Service dựa trên CPU/Memory Utilization.

- Kiểm thử thành công toàn bộ luồng: User → CloudFront → ALB → ECS Task → RDS/Redis.