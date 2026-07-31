---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

- Hoàn thiện hệ thống và xây dựng Workshop

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Hoàn thiện kiến trúc AWS <br> - Vẽ sơ đồ kiến trúc tổng thể bằng eraser.io | 27/07/2026 | 27/07/2026 | |
| 3 | - Thiết kế sơ đồ kiến trúc và bổ sung hình ảnh minh họa | 28/07/2026 | 28/07/2026 | |
| 4 | - Viết tài liệu Workshop bằng Hugo | 29/07/2026 | 29/07/2026 | <https://gohugo.io/> |
| 5, 6 | - Hoàn thiện phiên bản tiếng Việt và tiếng Anh <br> - Deploy Workshop lên GitHub Pages và kiểm tra lần cuối | 30/07/2026 | 30/07/2026 | |

### Kết quả đạt được tuần 8:

- Hoàn thiện sơ đồ kiến trúc triển khai hệ thống trên AWS bao gồm đầy đủ các thành phần:
  - CI/CD Pipeline: Developer → GitHub → GitHub Actions → Amazon ECR → Amazon ECS.
  - External Access Layer: Route 53 → CloudFront → AWS WAF → Internet Gateway → ALB.
  - Application Layer: ECS Fargate (NestJS) trong Private Application Subnet.
  - Data Layer: RDS PostgreSQL trong Private DB Subnet.
  - Supporting Services: S3, ElastiCache Redis, Secrets Manager, CloudWatch Logs, IAM Task Role.

- Hoàn thiện tài liệu Workshop theo đúng cấu trúc của FCAJ.

- Xây dựng đầy đủ:
  - Overview.
  - Prerequisite.
  - Architecture.
  - Step-by-step Guide.
  - Testing.
  - Clean-up.

- Bổ sung hình ảnh minh họa, sơ đồ kiến trúc và code snippet.

- Hoàn thiện phiên bản song ngữ (Tiếng Việt và Tiếng Anh).

- Deploy thành công Workshop lên GitHub Pages.

- Hoàn thiện báo cáo và chuẩn bị cho buổi demo dự án.