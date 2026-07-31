---
title : "Tự động hóa Pipeline CI/CD"
date : 2026-07-30
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Tự động hóa quá trình triển khai

Trong phần này, bạn sẽ xây dựng một pipeline **CI/CD (Continuous Integration & Continuous Deployment)** tự động bằng **GitHub Actions**. Pipeline này sẽ tự động build Docker Image cho cả ứng dụng **Backend (NestJS)** và **Frontend (Next.js)**, đẩy các Image lên **Amazon ECR**, sau đó tự động triển khai phiên bản mới lên **Amazon ECS Fargate** thông qua cơ chế **Rolling Update**, giúp cập nhật ứng dụng mà không làm gián đoạn dịch vụ.

#### Nội dung

- [1. Cấu hình IAM User và GitHub Secrets](5.6.1-iam-github-secrets/)
- [2. Tạo các Amazon ECR Repository](5.6.2-create-ecr/)
- [3. Thiết lập GitHub Actions Workflows](5.6.3-github-actions/)