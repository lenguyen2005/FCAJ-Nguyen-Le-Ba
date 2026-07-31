---
title: "Workshop"
date: 2026-07-31
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# EduShare: Nền tảng Chia sẻ Tri thức và Thúc đẩy Cộng đồng Học thuật Tiên tiến

#### Tổng quan

EduShare là một hệ thống trực tuyến hiện đại được thiết kế chuyên biệt cho việc lưu trữ, chia sẻ tài liệu và thảo luận học thuật. Không chỉ dừng lại ở một kho lưu trữ thông thường, EduShare kiến tạo một cộng đồng học tập sôi nổi thông qua việc tích hợp các tính năng tương tác thời gian thực (Hỏi đáp/Bình luận) và hệ thống Gamification (tích lũy điểm thưởng, thăng cấp, bảng xếp hạng) nhằm khơi dậy động lực đóng góp từ người dùng.

1. Điểm nhấn Kỹ thuật: Đề cao hiệu năng và khả năng mở rộng, EduShare được xây dựng dựa trên nền tảng Clean Architecture (NestJS) cho Backend và Feature-based Modular (Next.js) cho Frontend. Hệ thống vận hành toàn diện trên Hạ tầng điện toán đám mây AWS (AWS Cloud) với các đột phá:

2. Truyền tải dữ liệu siêu tốc: Cơ chế tải file trực tiếp lên Amazon S3 qua Presigned URL và phân phối nội dung toàn cầu thông qua mạng lưới Amazon CloudFront.

3. Hiệu năng & Mở rộng linh hoạt: Sử dụng Amazon ElastiCache (Redis) cho Bảng xếp hạng tức thời (Real-time Leaderboard) và triển khai Backend dưới dạng Serverless Container trên Amazon ECS (Fargate) giúp hệ thống tự động co giãn theo lưu lượng truy cập.

4. Bảo mật cấp độ Doanh nghiệp: Quản lý tập trung các thông tin nhạy cảm qua AWS Secrets Manager và tuân thủ nguyên tắc phân quyền tối thiểu (Least Privilege) của IAM.

EduShare không chỉ là một giải pháp giáo dục, mà còn là một minh chứng xuất sắc cho việc ứng dụng kiến trúc phần mềm hiện đại và hạ tầng Cloud để giải quyết các bài toán hệ thống quy mô lớn.


#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Hạ tầng mạng](5.3-VPC-Subnet/)
4. [Các dịch vụ](5.4-Service/)
5. [VPC Endpoint Policies (làm thêm)](5.5-Policy/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)