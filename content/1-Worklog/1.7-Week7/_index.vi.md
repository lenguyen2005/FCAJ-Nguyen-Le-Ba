---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

- Thiết lập bảo mật lớp ngoài: Amazon CloudFront, AWS WAF và Amazon Route 53
- Tích hợp Amazon S3 cho lưu trữ tài liệu và ảnh người dùng

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tạo CloudFront Distribution trỏ đến ALB <br> - Cấu hình Origin và Cache Behavior | 20/07/2026 | 20/07/2026 | <https://docs.aws.amazon.com/cloudfront/> |
| 3 | - Thiết lập AWS WAF Web ACL <br> - Gắn WAF vào CloudFront Distribution | 21/07/2026 | 21/07/2026 | <https://docs.aws.amazon.com/waf/> |
| 4 | - Cấu hình Amazon Route 53 domain <br> - Tạo SSL Certificate bằng ACM | 22/07/2026 | 22/07/2026 | <https://docs.aws.amazon.com/route53/> |
| 5 | - Tích hợp Amazon S3 lưu trữ Docs, Images, Avatars <br> - Cấu hình Presigned URL để upload an toàn | 23/07/2026 | 23/07/2026 | <https://docs.aws.amazon.com/s3/> |
| 6 | - Kiểm thử toàn bộ hệ thống End-to-End <br> - Thiết lập CloudWatch Logs và Alarms | 24/07/2026 | 24/07/2026 | |

### Kết quả đạt được tuần 7:

- Thiết lập thành công **Amazon CloudFront** làm CDN và điểm vào duy nhất cho toàn bộ traffic:
  - Origin trỏ đến ALB.
  - Cấu hình Cache Behavior riêng cho API và static assets.

- Tích hợp **AWS WAF** bảo vệ ứng dụng khỏi các tấn công phổ biến:
  - AWS Managed Rules: Core Rule Set, Known Bad Inputs.
  - Rate-based rule để chống DDoS cơ bản.

- Cấu hình **Amazon Route 53** với custom domain và SSL/TLS Certificate từ ACM.

- Tích hợp **Amazon S3** cho lưu trữ tài liệu và ảnh:
  - Bucket riêng cho Docs, Images và Avatars.
  - Sử dụng **Presigned URL** để người dùng upload file trực tiếp lên S3, giảm tải cho ECS.
  - Bucket Policy chỉ cho phép CloudFront đọc static assets.

- Thiết lập **Amazon CloudWatch**:
  - Log Group cho ECS Tasks.
  - Metric Alarms cho CPU, Memory và ALB 5xx errors.

- Hoàn thiện kiến trúc tổng thể và kiểm thử end-to-end thành công.