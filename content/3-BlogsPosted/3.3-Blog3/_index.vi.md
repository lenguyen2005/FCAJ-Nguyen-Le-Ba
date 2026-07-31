---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Tự dựng hệ thống Self-Managed RAG quy mô lớn với Amazon EKS và Amazon S3 Vectors

Trong quá trình tìm hiểu về các kiến trúc RAG (Retrieval-Augmented Generation) trên AWS, mình thấy nhiều doanh nghiệp dùng dịch vụ fully-managed như Amazon Bedrock Knowledge Bases để triển khai nhanh. Dịch vụ này rất tiện, giảm bớt tải vận hành hạ tầng ban đầu. Tuy nhiên, khi bài toán đi sâu vào thực tế và yêu cầu tùy biến chuyên sâu vào mô hình, tối ưu chi phí lưu trữ vector quy mô khổng lồ, hoặc cần chạy trên hạ tầng Kubernetes (EKS) có sẵn, giải pháp managed đôi khi bắt đầu bộc lộ những giới hạn về mặt linh hoạt lẫn chi phí.

Vì vậy, mình đã tìm hiểu một giải pháp thay thế: Tự dựng (Self-managed) hệ thống RAG trên Amazon EKS kết hợp với Amazon S3 Vectors và các công cụ Open Source đình đám như Ray, Hugging Face TGI, và LangChain.

---

## 1. Tổng quan kiến trúc và các bước triển khai

Kiến trúc này được thiết kế để chia tách rõ ràng thành 2 lớp chính: Knowledge Processing Layer đảm nhận việc số hóa dữ liệu batch quy mô lớn và Response Generation Layer xử lý truy vấn thời gian thực của người dùng.

### A. Luồng xử lý và số hóa dữ liệu
Ở lớp này, hệ thống tập trung vào việc đưa dữ liệu thô từ kho lưu trữ thành các không gian vector có thể truy vấn nhanh chóng:
* **Bước 1:** Upload toàn bộ tài liệu thô (file JSON, PDF, tài liệu nội bộ) vào S3 General Purpose Bucket đóng vai trò là Data Lake trung tâm.
* **Bước 2:** Kích hoạt một batch job chạy Ray Data trên Amazon EKS. Ray sẽ tiến hành đọc dữ liệu, phân tách tài liệu thành các đoạn nhỏ (chunking) và thực hiện tính toán phân tán hiệu năng cao trên các nút GPU/CPU của cụm Kubernetes.
* **Bước 3:** Sử dụng các mô hình embedding mã nguồn mở (như multilingual-e5-small hay Qwen3-Embedding) để biến đổi các đoạn văn bản thành các Vector Embeddings đa chiều.
* **Bước 4:** Ghi toàn bộ vector thu được cùng với metadata tương ứng vào Amazon S3 Vectors Bucket – kho lưu trữ vector chuyên dụng giúp query similarity tốc độ cao mà không cần quản lý hay duy trì bất kỳ database hạ tầng nào.

### B. Luồng truy vấn và sinh câu trả lời
Lớp này đóng vai trò tương tác trực tiếp với người dùng cuối thông qua giao diện chat:
* **Bước 5:** Người dùng gửi câu hỏi từ giao diện Chat (Streamlit) triển khai dưới dạng một container service trên cụm EKS.
* **Bước 6:** Sử dụng LangChain để chuyển câu hỏi của người dùng thành dạng vector (bằng chính model embedding đã dùng ở khâu ingestion), sau đó thực hiện vector similarity search trực tiếp vào S3 Vectors Bucket để lấy ra top-K văn bản liên quan nhất.
* **Bước 7:** LangChain đóng gói câu hỏi ban đầu cùng ngữ cảnh (context) vừa tìm được thành một contextualized prompt, gửi tới Inference Server (chạy Hugging Face TGI trên EKS với GPU).
* **Bước 8:** Mô hình LLM (như Mistral-7B, Llama-3, hoặc Qwen) sinh ra câu trả lời chính xác dựa trên dữ liệu doanh nghiệp và trả về giao diện người dùng kèm theo các nguồn tài liệu tham khảo (attribution).

---

## 2. Một vài điểm mình thấy hữu ích

* **S3 Vectors tối ưu chi phí vượt trội:** Đây là tính năng lưu trữ vector tích hợp thẳng vào S3. Không cần phải bật hay duy trì các cụm Vector Database đắt đỏ (như OpenSearch hay Pinecone) chạy 24/7. S3 Vectors tự động scale từ 0 lên hàng chục triệu vector với mô hình pay-per-use, giúp tiết kiệm ngân sách đáng kể cho các dự án tích trữ lượng dữ liệu khổng lồ.
* **Khả năng mở rộng mạnh mẽ nhờ Ray:** Thay vì xử lý từng file đơn lẻ theo dạng sự kiện tốn kém, Ray giúp phân tán công việc vector hóa dữ liệu khổng lồ trên cụm EKS một cách cực kỳ mượt mà. Ray hỗ trợ tự động xử lý lỗi (fault tolerance), quản lý bộ nhớ tốt và phân bổ tài nguyên GPU/CPU thông minh cho cả dạng xử lý batch lẫn streaming dữ liệu sau này.
* **Toàn quyền kiểm soát:** Việc dùng Hugging Face TGI (hoặc vLLM) để serve LLM giúp chủ động chọn bất kỳ model mã nguồn mở nào tùy theo bài toán, tự do tùy chỉnh các tham số kỹ thuật quan trọng (quantization, tensor parallelism, PagedAttention) hoặc thậm chí dễ dàng triển khai ở các vùng (Region) chưa được AWS hỗ trợ các dịch vụ AI fully-managed.

---

## 3. Kết luận

Kiến trúc Self-Managed RAG kết hợp Amazon EKS + S3 Vectors là một giải pháp cực kỳ mạnh mẽ cho các đội ngũ đã có kinh nghiệm làm việc với Kubernetes và Open Source AI. Nó xóa bỏ rào cản chi phí lưu trữ vector ở quy mô lớn, đồng thời giữ trọn quyền kiểm soát hạ tầng, mô hình và luồng xử lý dữ liệu cho doanh nghiệp.

---

## 4. Tài liệu tham khảo

* [AWS Storage Blog – Building self-managed RAG applications with Amazon EKS and Amazon S3 Vectors](https://aws.amazon.com/blogs/storage/building-self-managed-rag-applications-with-amazon-eks-and-amazon-s3-vectors/)
* [Amazon EKS Documentation](https://docs.aws.amazon.com/eks/)
* [Ray on Kubernetes (KubeRay)](https://docs.ray.io/en/latest/cluster/kubernetes/index.html)

<!-- ### 🔗 Bài viết đã đăng trên Facebook Group
👉 **Link bài viết:** [AWS Study Group FB](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2227063374725289/) -->