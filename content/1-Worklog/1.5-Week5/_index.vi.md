---
title: "Worklog Tuần 5"
date: 2026-7-11
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

- Xây dựng ứng dụng NestJS và đóng gói bằng Docker
- Thiết lập Amazon ECR và CI/CD pipeline với GitHub Actions

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Khởi tạo project NestJS <br> - Xây dựng cấu trúc module cơ bản (Auth, User, Document) | 6/07/2026 | 6/07/2026 | <https://docs.nestjs.com/> |
| 3   | - Viết Dockerfile multi-stage cho NestJS <br> - Test build image local | 7/07/2026 | 7/07/2026 | |
| 4   | - Tạo Amazon ECR Repository <br> - Đẩy Docker image lên ECR lần đầu | 8/07/2026 | 8/07/2026 | <https://docs.aws.amazon.com/ecr/> |
| 5-6 | - Thiết lập GitHub Actions Workflow: build → push ECR → deploy ECS <br> - Cấu hình IAM Role cho GitHub Actions (OIDC) | 9/07/2026 | 10/07/2026 | <https://docs.github.com/en/actions> |

### Kết quả đạt được tuần 5:

- Khởi tạo thành công project **NestJS** với cấu trúc module rõ ràng theo Domain-Driven Design.

- Hiểu được cách tổ chức code theo module trong NestJS:
  - Module Auth: đăng ký, đăng nhập, JWT.
  - Module User: quản lý hồ sơ người dùng.
  - Module Document: tải lên, tìm kiếm, quản lý tài liệu.

- Xây dựng thành công **Dockerfile multi-stage**:
  - Stage 1 (builder): cài đặt dependencies và build TypeScript.
  - Stage 2 (production): copy dist và node_modules, image nhỏ gọn.

- Hiểu được quy trình làm việc với **Amazon ECR**:
  - Tạo Private Repository.
  - Authenticate Docker với ECR bằng AWS CLI.
  - Tag và push image lên ECR.

- Thiết lập **GitHub Actions CI/CD Pipeline** hoàn chỉnh:
  - Trigger on push to `main` branch.
  - Build Docker image.
  - Push image lên Amazon ECR.
  - Cập nhật ECS Service với image tag mới.

- Cấu hình **IAM OIDC** cho GitHub Actions để xác thực an toàn mà không cần lưu AWS credentials trong secrets.