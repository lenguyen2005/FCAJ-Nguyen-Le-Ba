---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
## Secure Multi-Tenant RAG với Amazon Bedrock và Verified Permissions

Trong quá trình tìm hiểu về các kiến trúc **RAG (Retrieval-Augmented Generation)** trên AWS, mình có gặp một bài toán khá phổ biến trong các doanh nghiệp: **Làm sao để dựng một ứng dụng AI hỏi đáp phục vụ nhiều phòng ban, nhưng vẫn đảm bảo nhân viên phòng nào chỉ xem được tài liệu của phòng đó?**

Giải pháp đơn giản nhất chắc có lẽ là tạo mỗi phòng ban một Amazon Bedrock Knowledge Base riêng biệt. Tuy nhiên, cách làm này sẽ làm tăng chi phí hạ tầng và cồng kềnh trong vận hành. Vì vậy mình đã tìm hiểu về một giải pháp thay thế: **kết hợp Amazon Bedrock Knowledge Bases và Amazon Verified Permissions để phân quyền truy cập tài liệu ngay trên một Knowledge Base dùng chung duy nhất.**

---

## 1. Tổng quan các bước triển khai

Để hiểu rõ hơn cách hệ thống hoạt động, mô hình này áp dụng nguyên tắc **Defense in Depth** với hai lớp kiểm tra độc lập:

### A. Luồng nạp dữ liệu
* **Bước 1:** Upload file PDF vào Amazon S3 theo cấu trúc thư mục phòng ban (ví dụ: `docs/dept-a/report.pdf`).
* **Bước 2:** Amazon EventBridge trigger SQS và AWS Lambda để tự động tạo một file gắn thẻ `report.pdf.metadata.json` chứa thông tin phòng ban (sidecar metadata).
* **Bước 3:** Lên lịch chạy Ingestion Job để Bedrock Knowledge Base đọc file cùng với metadata sidecar và index vào Vector Database.

### B. Luồng truy vấn
* **Bước 4:** Người dùng gửi câu hỏi qua Amazon API Gateway. Một Lambda Authorizer gọi đến Amazon Verified Permissions để kiểm tra xem user có quyền gọi API hay không (Lớp 1).
* **Bước 5:** Nếu lớp 1 được thông qua, Middleware Lambda tiếp tục gọi Verified Permissions để hỏi xem user này được phép đọc tài liệu của những phòng ban nào.
* **Bước 6:** Middleware Lambda xây dựng bộ lọc `kb_filter` (Metadata Filter) và truyền vào API `RetrieveAndGenerate` của Amazon Bedrock.
* **Bước 7:** Bedrock chỉ tìm kiếm và đưa các đoạn văn bản thỏa mãn bộ lọc cho LLM để sinh câu trả lời.
* **Bước 8:** Áp dụng **Guardrails for Amazon Bedrock** để kiểm tra độ tin cậy của câu trả lời trước khi trả về cho người dùng (Lớp 2).

---

## 2. Một vài điểm mình thấy hữu ích

* **Tối ưu chi phí & hạ tầng:** Gom tất cả tài liệu vào 1 Knowledge Base duy nhất thay vì tạo hàng chục instance độc lập cho từng nhóm.
* **Quản lý chính sách tách biệt:** Toàn bộ luật phân quyền được viết bằng ngôn ngữ Cedar trong Amazon Verified Permissions. Đội ngũ Security có thể quản lý policy mà không cần can thiệp vào mã nguồn backend.
* **An toàn hai lớp:** Lớp 1 chặn các truy cập API trái phép, Lớp 2 lọc dữ liệu ở cấp độ vector search. Kể cả khi lớp 1 bị cấu hình sai, lớp thứ 2 vẫn đảm bảo LLM không bao giờ "thấy" được tài liệu bị cấm.
* **Cập nhật tức thì:** Thay đổi policy trên Verified Permissions sẽ có hiệu lực ngay ở request tiếp theo mà không cần khởi động lại dịch vụ.

---

## 3. Kết luận

Đối với mình, đây là một mô hình kiến trúc RAG nâng cao rất đáng để tham khảo khi làm việc với ứng dụng AI trong doanh nghiệp. Việc tách biệt logic phân quyền ra khỏi ứng dụng RAG không chỉ giúp code gọn gàng hơn mà còn mang lại khả năng kiểm soát an toàn dữ liệu chuẩn mực.

---

## 4. Tài liệu tham khảo

* [AWS Machine Learning Blog – Secure multi-tenant RAG with Amazon Bedrock and Verified Permissions](https://aws.amazon.com/blogs/machine-learning/secure-multi-tenant-rag-with-amazon-bedrock-and-verified-permissions/)
* [Amazon Bedrock Knowledge Bases Documentation](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html)
* [Amazon Verified Permissions Developer Guide](https://docs.aws.amazon.com/verifiedpermissions/latest/userguide/what-is-avp.html)

---
<!-- 
### 🔗 Bài viết đã đăng trên Facebook Group
👉 **Link bài viết:** [AWS Study Group FB](https://www.facebook.com/groups/awsstudygroupfcj/posts/2224218628343097/) -->
