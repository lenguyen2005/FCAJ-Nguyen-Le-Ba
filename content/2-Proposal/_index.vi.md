---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# EduShare
## Nền tảng chia sẻ tài liệu học tập triển khai trên AWS ECS Fargate

---

### 1. Tóm tắt điều hành

**EduShare** là một nền tảng web cho phép sinh viên, học viên và giảng viên chia sẻ tài liệu học tập một cách an toàn và hiệu quả. Hệ thống backend được xây dựng bằng **NestJS**, đóng gói thành Docker container và triển khai trên **Amazon ECS Fargate**, đảm bảo khả năng mở rộng linh hoạt và không cần quản lý hạ tầng máy chủ.

Kiến trúc tận dụng các dịch vụ AWS để đảm bảo:
- **Bảo mật**: AWS WAF, Secrets Manager, VPC Private Subnet.
- **Hiệu năng**: CloudFront CDN, ElastiCache Redis, App Auto Scaling.
- **Tự động hóa**: CI/CD pipeline với GitHub Actions và Amazon ECR.

---

### 2. Tuyên bố vấn đề

**Vấn đề hiện tại**

Sinh viên và giảng viên hiện nay gặp khó khăn trong việc chia sẻ và tìm kiếm tài liệu học tập do thiếu một nền tảng tập trung, bảo mật và dễ dùng. Tài liệu bị phân tán trên nhiều kênh khác nhau (email, nhóm chat, cloud storage cá nhân) khiến việc tìm kiếm và quản lý trở nên tốn thời gian.

**Giải pháp**

EduShare cung cấp một nền tảng web tập trung nơi người dùng có thể:
- Đăng ký, đăng nhập bằng JWT Authentication.
- Upload tài liệu (PDF, DOCX, PPTX) lên Amazon S3 qua Presigned URL.
- Tìm kiếm và tải tài liệu từ những người dùng khác.
- Quản lý hồ sơ cá nhân và avatar.

---

### 3. Kiến trúc giải pháp

Kiến trúc bao gồm 4 lớp chính:

**Lớp CI/CD (CI/CD Pipeline)**
- Developer push code lên GitHub Repository.
- GitHub Actions tự động build Docker image, push lên Amazon ECR và deploy lên Amazon ECS.

**Lớp tiếp cận bên ngoài (External Access)**
- Người dùng truy cập qua Amazon Route 53 (custom domain).
- Traffic đi qua Amazon CloudFront (CDN) → AWS WAF (bảo vệ) → Internet Gateway → Application Load Balancer.
- Upload file lớn đi trực tiếp lên S3 qua Presigned URL.

**Lớp ứng dụng (Application Layer - Amazon VPC)**
- Public Subnet: Application Load Balancer, NAT Gateway.
- Private Application Subnet: Amazon ECS Cluster chạy Fargate Tasks (NestJS), có App Auto Scaling.
- Private DB Subnet: Amazon RDS PostgreSQL (Single-AZ).

**Lớp lưu trữ và hỗ trợ (Storage & Supporting Services)**
- Amazon S3: Lưu trữ tài liệu, hình ảnh, avatar.
- ElastiCache Redis: Cache session và dữ liệu truy vấn thường xuyên.
- AWS Secrets Manager: Quản lý credentials (DB password, JWT secret).
- Amazon CloudWatch Logs: Giám sát và ghi log hệ thống.
- IAM Task Role: Phân quyền cho ECS container truy cập các dịch vụ AWS.

---

### 4. Triển khai kỹ thuật

| Thành phần | Công nghệ |
| ---------- | --------- |
| Backend Framework | NestJS (TypeScript) |
| Container Runtime | Docker (multi-stage build) |
| Container Orchestration | Amazon ECS Fargate |
| Container Registry | Amazon ECR |
| CI/CD | GitHub Actions + IAM OIDC |
| Database | Amazon RDS PostgreSQL (Single-AZ) |
| Cache | ElastiCache Redis |
| File Storage | Amazon S3 + Presigned URL |
| CDN & Edge | Amazon CloudFront |
| Security | AWS WAF, Secrets Manager, VPC |
| DNS | Amazon Route 53 + ACM SSL |
| Monitoring | Amazon CloudWatch Logs & Alarms |

---

### 5. Lộ trình & Mốc triển khai

| Tuần | Giai đoạn | Nội dung |
| ---- | --------- | -------- |
| Tuần 4 | Thiết kế | Chốt đề tài, thiết kế kiến trúc, học VPC và ECS cơ bản |
| Tuần 5 | Phát triển Backend | Xây dựng NestJS, Dockerfile, ECR, GitHub Actions CI/CD |
| Tuần 6 | Triển khai hạ tầng | ECS Fargate, ALB, RDS, Redis, Secrets Manager |
| Tuần 7 | Bảo mật & Tích hợp | CloudFront, WAF, Route 53, S3 Presigned URL |
| Tuần 8 | Hoàn thiện | Workshop, tài liệu, demo và deploy lên GitHub Pages |

---

### 6. Ước tính ngân sách

| Dịch vụ | Chi phí ước tính (USD/tháng) |
| ------- | --------------------------- |
| Amazon ECS Fargate (0.5 vCPU, 1 GB) | ~15 |
| Amazon RDS PostgreSQL db.t3.micro | ~15 |
| ElastiCache Redis cache.t3.micro | ~12 |
| Amazon CloudFront (10 GB transfer) | ~1 |
| Amazon S3 (10 GB storage) | ~0.23 |
| Application Load Balancer | ~16 |
| NAT Gateway | ~32 |
| **Tổng ước tính** | **~91 USD/tháng** |

---

### 7. Đánh giá rủi ro

| Rủi ro | Mức độ | Giải pháp |
| ------ | ------ | --------- |
| RDS Single-AZ gặp sự cố | Trung bình | Snapshot tự động hàng ngày, có thể nâng cấp lên Multi-AZ |
| ECS Task crash | Thấp | ECS Service tự động khởi động lại Task |
| Chi phí vượt ngân sách | Thấp | CloudWatch Budget Alarm |
| Bảo mật credentials | Thấp | AWS Secrets Manager + IAM Task Role |

---

### 8. Kết quả kỳ vọng

- Hệ thống backend NestJS chạy ổn định trên Amazon ECS Fargate với auto scaling.
- CI/CD pipeline tự động từ GitHub đến ECS trong vòng dưới 5 phút.
- Người dùng có thể đăng ký, đăng nhập, upload và tải tài liệu thành công.
- Tài liệu Workshop hoàn chỉnh bằng tiếng Việt và tiếng Anh.
