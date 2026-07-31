---
title: "Worklog Tuần 4"
date: 2026-07-04
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---



### Mục tiêu tuần 4:
- Thống nhất ý tưởng và chốt đề tài xây dụng project
- Tìm hiểu thêm về S3 bucket, AWS Bedrock

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm kiếm và chốt đề tài cho dự án cuối khóa.                                                                                                               | 29/06/2026   | 29/06/2026      |
| 3   | - Xác định các chức năng chính, phân tích người dùng.                                                                                                                                       | 30/06/2026   | 30/06/2026      |  |
| 4   | - Nghiên cứu dịch vụ S3 Bucket                                                                                                                                                              | 1/07/2026    | 1/07/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu về Amazon Bedrock                                                                                                                                                                | 2/07/2026    | 2/07/2026      | <https://docs.aws.amazon.com/bedrock/> |
| 6   | - **Thực hành:** <br>&emsp; + Tạo Tạo Bucket <br>&emsp; + Upload Object <br>&emsp; + Download Object<br>&emsp; + Xóa Object <br>&emsp;....                                                  | 3/07/2026    | 3/07/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 4:

- Hoàn thành việc khảo sát chốt được nội dung cho dự án EduShare.

- Xác định được bài toán và phạm vi của dự án:
  - Đối tượng sử dụng là học sinh, phụ huynh và giáo viên có nhu cầu tìm hiểu thông tin tuyển sinh.
  - Mục tiêu xây dựng hệ thống hỏi đáp thông minh sử dụng LLM kết hợp kỹ thuật Retrieval-Augmented Generation (RAG).
  - Xác định các chức năng chính của hệ thống như:
    - Trả lời câu hỏi về tuyển sinh.
    - Tra cứu phương thức xét tuyển.
    - Tra cứu học phí, chỉ tiêu và ngành đào tạo.
    - Trích dẫn nguồn tài liệu khi trả lời.

- Hiểu được vai trò của **Amazon S3** trong việc lưu trữ dữ liệu trên AWS.

- Nắm được các khái niệm cơ bản của Amazon S3:
  - Bucket
  - Object
  - Key
  - Storage Class

- Biết cách tạo và quản lý S3 Bucket thông qua AWS Management Console.

- Thực hiện các thao tác cơ bản với Amazon S3:
  - Tạo Bucket
  - Upload Object
  - Download Object
  - Xóa Object
  - Cấu hình Bucket Policy và Block Public Access

- Hiểu được vai trò của Amazon S3 trong hệ thống RAG, sử dụng làm nơi lưu trữ tài liệu tuyển sinh trước khi xây dựng Knowledge Base.

- Tìm hiểu về **Amazon Bedrock** và các mô hình nền tảng (Foundation Models) do AWS cung cấp.

- Hiểu được cách Amazon Bedrock hỗ trợ:
  - Text Generation
  - Chatbot
  - Embedding
  - Retrieval-Augmented Generation (RAG)

- Nắm được quy trình tích hợp Amazon Bedrock vào hệ thống AI:
  - Người dùng gửi câu hỏi.
  - Hệ thống truy xuất tài liệu liên quan.
  - Amazon Bedrock sinh câu trả lời dựa trên ngữ cảnh được cung cấp.

- Bước đầu xây dựng kiến trúc tổng thể cho dự án chatbot tuyển sinh trên AWS sử dụng các dịch vụ:
  - Amazon S3
  - AWS Lambda
  - Amazon API Gateway
  - Amazon Bedrock
  - Amazon OpenSearch Serverless (Vector Database)
  - Amazon CloudWatch

- **Thực hành Amazon EC2:**
  - Khởi tạo EC2 Instance từ Amazon Machine Image (AMI).
  - Cấu hình Security Group và Key Pair.
  - Kết nối EC2 thông qua SSH.
  - Tạo và gắn (Attach) Amazon EBS Volume vào EC2 Instance.
  - Thực hiện mount EBS Volume và kiểm tra khả năng lưu trữ trên hệ điều hành Linux.
  - Hiểu được mối quan hệ giữa EC2 và EBS cũng như các trường hợp sử dụng trong thực tế.

- Đánh giá được tính khả thi của dự án và định hướng lựa chọn kiến trúc **Serverless** nhằm tối ưu chi phí, khả năng mở rộng và giảm công sức quản trị hạ tầng.