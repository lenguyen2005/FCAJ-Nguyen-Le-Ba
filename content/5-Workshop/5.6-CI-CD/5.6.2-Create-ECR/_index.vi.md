---
title : "Tạo các Amazon ECR Repository"
date : 2026-07-30
weight : 2
chapter : false
pre : " <b> 5.6.2 </b> "
---

#### Tạo các ECR Repository

Chúng ta cần tạo hai **Elastic Container Registry (ECR)** riêng tư (Private) để lưu trữ các Docker Image của ứng dụng Backend và Frontend.

1. Truy cập **Amazon ECR Console**.
2. Trong thanh điều hướng bên trái, chọn **Repositories**, sau đó nhấn **Create repository**.

![ecr-create](/images/5-Workshop/5.6-CICD/create_ecr.png)

3. Tạo Repository cho **Backend**:
   + **Visibility settings:** **Private**
   + **Repository name:** `edushare-backend`
   + Giữ nguyên các thiết lập mặc định khác và nhấn **Create repository**.

4. Tạo Repository cho **Frontend**:
   + Nhấn **Create repository** một lần nữa.
   + **Visibility settings:** **Private**
   + **Repository name:** `edushare-frontend`
   + Nhấn **Create repository**.

| Tên Repository | Chế độ hiển thị | Vai trò |
| :--- | :--- | :--- |
| `edushare-backend` | Private | Lưu trữ Docker Image của API NestJS |
| `edushare-frontend` | Private | Lưu trữ Docker Image của giao diện Next.js |

5. Ghi lại **URI** của từng Repository (ví dụ: `123456789012.dkr.ecr.us-east-1.amazonaws.com/edushare-backend`). URI này sẽ được sử dụng trong các workflow của GitHub Actions.