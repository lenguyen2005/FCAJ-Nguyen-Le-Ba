---
title : "Prerequisites"
date : 2026-07-30 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

#### Quyền hạn IAM
Để triển khai và dọn dẹp các tài nguyên trong workshop EduShare, tài khoản IAM user của bạn cần có đủ quyền hạn. Trong môi trường thực hành, bạn có thể gán chính sách `AdministratorAccess` cho tiện lợi. Nếu muốn bảo mật hơn, hãy gán policy tùy chỉnh sau đây bao gồm các dịch vụ chính được sử dụng:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EduShareWorkshopPermissions",
            "Effect": "Allow",
            "Action": [
                "ec2:*",
                "ecs:*",
                "ecr:*",
                "elasticloadbalancing:*",
                "rds:*",
                "elasticache:*",
                "s3:*",
                "secretsmanager:*",
                "iam:*",
                "logs:*"
            ],
            "Resource": "*"
        }
    ]
}

```

#### Chuẩn bị Môi trường & Khu vực (Region)

Trong lab này, chúng ta sẽ dùng N.Virginia region (us-east-1).

Trong workshop này, chúng ta sẽ dùng N.Virginia region (us-east-1) để đảm bảo tính sẵn có và độ ổn định của các dịch vụ.

Workshop này tập trung vào việc triển khai thủ công từng bước qua giao diện AWS Management Console nhằm giúp bạn hiểu sâu về kiến trúc mạng. Trước khi bắt đầu, hãy đảm bảo bạn đã chuẩn bị:

![create stack](/images/5-Workshop/5.2-Prerequisite/region.png)

+ **AWS Region**: Chuyển khu vực trên Console của bạn sang **us-east-1** (góc trên cùng bên phải).
+ **Mã nguồn**: Đã tải sẵn mã nguồn của EduShare Frontend (Next.js) và Backend (NestJS)
+ **Tài khoản GitHub**: Cần thiết để thiết lập luồng tự động hóa CI/CD bằng GitHub Actions ở các phần sau.
