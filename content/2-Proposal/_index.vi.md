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

**EduShare** là nền tảng chia sẻ tài liệu học tập được phát triển theo mô hình Fullstack. Hệ thống gồm frontend được xây dựng bằng **Next.js** và backend được xây dựng bằng **NestJS**. Backend được đóng gói thành Docker container và triển khai trên **Amazon ECS Fargate**, trong khi frontend được triển khai riêng, đảm bảo khả năng mở rộng linh hoạt và không cần quản lý hạ tầng máy chủ.

Kiến trúc triển khai sử dụng Amazon ECS Fargate, Amazon RDS PostgreSQL, Amazon S3, Amazon CloudFront, AWS WAF, Amazon ElastiCache Redis, AWS Secrets Manager và GitHub Actions để xây dựng một nền tảng chia sẻ tài liệu học tập có khả năng mở rộng, bảo mật và tự động hóa triển khai.

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
- Phân loại tài liệu theo danh mục.
- Hệ thống Gamification (điểm EXP, Level, Leaderboard).

---

### 3. Kiến trúc giải pháp

Kiến trúc bao gồm 4 lớp chính:

**Lớp CI/CD (CI/CD Pipeline)**
- Developer push code lên GitHub Repository.
- GitHub Actions tự động build Docker image, push lên Amazon ECR và deploy lên Amazon ECS.

**Lớp tiếp cận bên ngoài (External Access)**
- Người dùng truy cập qua Amazon Route 53 (custom domain).
- Traffic đi qua Amazon CloudFront (CDN) → AWS WAF (bảo vệ) → Application Load Balancer → Amazon ECS Fargate.
- Frontend yêu cầu Backend tạo Presigned URL, sau đó trình duyệt upload trực tiếp lên Amazon S3 nhằm giảm tải cho ECS và tăng hiệu năng.

**Lớp ứng dụng (Application Layer - Amazon VPC)**
- Public Subnet: Application Load Balancer, NAT Gateway.
- Private Application Subnet: Amazon ECS Cluster chạy ECS Service và Fargate Tasks (NestJS), hỗ trợ App Auto Scaling.
- Private DB Subnet: Amazon RDS PostgreSQL.

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
| Frontend Framework | Next.js (React + TypeScript) |
| Container Runtime | Docker (multi-stage build) |
| Container Orchestration | Amazon ECS Fargate |
| Container Registry | Amazon ECR |
| CI/CD | GitHub Actions + IAM OIDC |
| Database | Amazon RDS PostgreSQL |
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
| ECS Task crash | Thấp | ECS Service tự động khởi động lại Task |
| Chi phí vượt ngân sách | Thấp | CloudWatch Budget Alarm |
| Bảo mật credentials | Thấp | AWS Secrets Manager + IAM Task Role |

---

### 8. Kết quả kỳ vọng

- Backend NestJS được container hóa bằng Docker và triển khai thành công trên Amazon ECS Fargate.
- CI/CD Pipeline tự động build, push image lên Amazon ECR và triển khai lên ECS sau mỗi lần merge vào nhánh chính.
- Hệ thống hỗ trợ upload tài liệu trực tiếp lên Amazon S3 bằng Presigned URL.
- Ứng dụng được phân phối thông qua CloudFront và bảo vệ bởi AWS WAF.
- Dữ liệu được lưu trữ trên Amazon RDS PostgreSQL và cache bằng ElastiCache Redis.
- Hệ thống có khả năng giám sát bằng Amazon CloudWatch và mở rộng theo tải nhờ ECS Service Auto Scaling.
- Tài liệu Workshop hoàn chỉnh bằng tiếng Việt và tiếng Anh.
