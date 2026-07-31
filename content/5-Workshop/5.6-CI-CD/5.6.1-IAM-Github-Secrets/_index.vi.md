---
title : "IAM User & GitHub Secrets"
date : 2026-07-30
weight : 1
chapter : false
pre : " <b> 5.6.1 </b> "
---

#### Tạo IAM User cho GitHub Actions

Để GitHub Actions có thể đẩy Docker Image lên Amazon ECR và cập nhật dịch vụ Amazon ECS một cách an toàn, chúng ta cần tạo một IAM User chuyên biệt với quyền truy cập thông qua Access Key.

1. Truy cập **Amazon IAM Console**.
2. Trong thanh điều hướng bên trái, chọn **IAM Users**, sau đó nhấn **Create user**.
   + **User name:** `github-actions-deployer`
   + **Không** chọn **Provide user access to the AWS Management Console**, sau đó nhấn **Next**.

![iam-user](/images/5-Workshop/5.6-CI-CD/create_iam_user.png)

3. Tại trang **Set permissions**, chọn **Attach policies directly**.
   + Tìm và chọn các Managed Policy sau:
     + `AmazonEC2ContainerRegistryPowerUser` (cho phép đẩy Docker Image lên Amazon ECR)
     ![ECR-permission](/images/5-Workshop/5.6-CI-CD/ECR_permission.png)
     + `AmazonECS_FullAccess` (cho phép kích hoạt triển khai mới trên Amazon ECS)
     ![ECS-permission](/images/5-Workshop/5.6-CI-CD/ECS_permission.png)
   + Nhấn **Next**, sau đó chọn **Create user**.
   ![create](/images/5-Workshop/5.6-CI-CD/create_user.png)

4. Tạo Access Key:
   + Chọn IAM User vừa tạo `github-actions-deployer`.
   + Chuyển sang tab **Security credentials**.
   + Trong mục **Access keys**, nhấn **Create access key**.
   + Chọn **Third-party service**, đánh dấu vào ô xác nhận, sau đó nhấn **Next**.
   ![create](/images/5-Workshop/5.6-CI-CD/create_access_key.png)
   + Sau đó nhấn **Create Access Key**.
   + Sao chép **Access key ID** và **Secret access key**, đồng thời lưu trữ chúng ở nơi an toàn.

#### Cấu hình GitHub Secrets

1. Truy cập repository của dự án trên GitHub.
2. Điều hướng đến **Settings** → **Secrets and variables** → **Actions**.
3. Chọn **New repository secret** và thêm các Secret sau:

   + `AWS_ACCESS_KEY_ID`: Dán **Access Key ID** vừa tạo.
   + `AWS_SECRET_ACCESS_KEY`: Dán **Secret Access Key** tương ứng.
   + `NEXT_PUBLIC_API_URL`: Địa chỉ DNS của **Application Load Balancer (ALB)** (ví dụ: `https://api.edushare.com`) được sử dụng trong quá trình build Frontend.

![github-secrets](/images/5-Workshop/5.6-CI-CD/github_secrets.png)