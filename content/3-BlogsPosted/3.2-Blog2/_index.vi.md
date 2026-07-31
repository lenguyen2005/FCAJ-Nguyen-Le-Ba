---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Tự động mở rộng dung lượng cho Amazon RDS Multi-AZ DB Cluster bằng AWS Lambda

Trong quá trình quản lý và vận hành hệ thống cơ sở dữ liệu trên AWS, một trong những sự cố nghiêm trọng nhất mà các DBA hay DevOps gặp phải là RDS bị cạn kiệt dung lượng lưu trữ.. Khi đĩa bị đầy 100%, cơ sở dữ liệu sẽ lập tức rơi vào trạng thái ngừng hoạt động (Inaccessible-storage), làm gián đoạn toàn bộ ứng dụng, hỏng các transaction đang xử lý dở dang và tiêu tốn rất nhiều thời gian để phục hồi.

Mặc dù tính năng Storage Auto-scaling có sẵn của Amazon RDS hoạt động rất tốt trên các instance đơn hoặc Multi-AZ tiêu chuẩn, nhưng Amazon RDS Multi-AZ DB clusters lại chưa hỗ trợ sẵn tính năng tự động này. Việc phải theo dõi thủ công, sau đó vào console gửi lệnh modify instance và tốn từ vài chục phút đến vài giờ để chờ áp dụng không chỉ gây áp lực vận hành mà còn rất rủi ro nếu sự cố xảy ra ngoài giờ làm việc.

Vì vậy, mình đã tìm hiểu một giải pháp tự động hóa gọn nhẹ nhưng rất hiệu quả: kết hợp Amazon CloudWatch, AWS Lambda và Amazon SNS để xây dựng cơ chế tự động mở rộng dung lượng đĩa cho RDS Multi-AZ Cluster ngay khi chạm ngưỡng cảnh báo.

---
## 1. Tổng quan kiến trúc và các bước triển khai

Hệ thống được thiết kế theo cơ chế phản ứng dựa trên sự kiện (event-driven architecture), đảm bảo tính chủ động, phản ứng tức thì và sử dụng hoàn toàn các dịch vụ native của AWS mà không cần cài đặt thêm công cụ từ bên thứ ba.
![Tổng quan kiến trúc](/images/3-Blog/3.2-Blog2/blog2.jpg)
### A. Luồng giám sát và phát hiện
* **Bước 1:** Thiết lập một CloudWatch Alarm để theo dõi sát sao metric `FreeStorageSpace` của từng RDS DB Instance trong cụm Cluster.
* **Bước 2:** Cấu hình ngưỡng kích hoạt an toàn. Mức khuyến nghị thường là khi dung lượng đĩa trống còn lại dưới 15% tổng dung lượng ban đầu (ví dụ: ổ 100 GB thì còn 15 GB trống sẽ báo động).

### B. Luồng xử lý & Tự động mở rộng
* **Bước 3:** Ngay khi đĩa chạm ngưỡng, CloudWatch Alarm chuyển sang trạng thái ALARM và tự động kích hoạt hàm AWS Lambda thông qua Resource-based Policy.
* **Bước 4:** Hàm Lambda nhận thông tin sự kiện, truy vấn trực tiếp AWS SDK (Boto3) để lấy chi tiết cấu hình hiện tại của DB Cluster (như `AllocatedStorage`, loại ổ đĩa gp3, io1, io2, hay thông số IOPS/Throughput).
* **Bước 5:** Lambda tính toán dung lượng mới dựa trên tham số `SCALING_PERCENTAGE` được cấu hình qua biến môi trường (mặc định tăng 15% để tối ưu chi phí, hoặc 30%–40% nếu hệ thống có tốc độ ghi dữ liệu cực nhanh).
* **Bước 6:** Lambda thực thi hàm `modify_db_cluster` với tùy chọn `ApplyImmediately = True` để tiến hành nâng cấp kích thước đĩa ngay lập tức, đồng thời tự động bảo toàn các thông số hiệu năng đi kèm.

### C. Luồng thông báo
* **Bước 7:** Đồng thời với việc gọi Lambda, CloudWatch Alarm gửi một bản tin cảnh báo qua Amazon SNS tới Email hoặc kênh Slack của đội ngũ vận hành để mọi người nắm bắt được sự kiện tự động mở rộng đĩa đang diễn ra.

---

## 2. Một vài điểm kỹ thuật mình thấy rất hay và hữu ích

* **Xử lý thông minh các loại ổ đĩa (IOPS & Throughput):** Điểm sáng của đoạn mã Lambda trong giải pháp này là khả năng nhận biết loại đĩa lưu trữ. Nếu bạn dùng đĩa gp3, Lambda sẽ giữ nguyên IOPS cơ sở (3000) và Throughput. Nếu dùng đĩa Provisioned IOPS (io1/io2), Lambda sẽ tự động tính toán lại và truyền tham số IOPS bắt buộc của AWS khi thay đổi size đĩa, tránh việc request bị thất bại do thiếu thông số.
* **Hoàn toàn tự động hóa không cần can thiệp thủ công:** Không còn kịch bản kỹ thuật viên phải nhận cảnh báo lúc 2 giờ sáng, bật máy tính lên console gõ lệnh và ngồi chờ. Hệ thống tự động phát hiện và thực thi mở rộng trong vài giây.
* **Triển khai quy mô lớn bằng CloudFormation:** Bài viết cung cấp sẵn CloudFormation template cho phép bạn quản lý cùng lúc hàng chục hay hàng trăm DB Instance chỉ bằng cách truyền danh sách ID và ngưỡng tương ứng (`prod-db-1`, `prod-db-2`). Điều này rất tiện cho các môi trường Enterprise có nhiều tài nguyên database.

---

## 3. Một số lưu ý quan trọng trước khi áp dụng

* **Quy định thời gian giãn cách của AWS RDS:** Amazon RDS quy định bắt buộc phải có khoảng nghỉ tối thiểu 6 giờ giữa 2 lần sửa đổi dung lượng lưu trữ trên cùng một DB. Điều này có nghĩa là nếu bạn chỉ scale tăng thêm 10% nhưng dữ liệu phình ra quá nhanh trong vòng 1-2 tiếng sau đó, Lambda sẽ không thể scale tiếp lần 2. Do đó, hãy chọn phần trăm tăng đủ an toàn từ 20%–30%.
* **Chi phí lưu trữ chỉ tăng, không giảm:** Dung lượng đĩa trên RDS sau khi đã nâng lên thì không thể thu nhỏ lại. Mọi chi phí lưu trữ sẽ tính theo dung lượng mới vĩnh viễn. Cần tính toán kỹ mức tăng để tránh lãng phí ngân sách.
* **Giới hạn tối đa của Engine Database:** Mỗi loại DB Engine và Instance class đều có giới hạn đĩa tối đa, ví dụ 64 TB hoặc 128 TB. Giải pháp này hiện tại chưa tích hợp đoạn code check trần dung lượng tối đa, bạn nên lưu ý giới hạn này.

---

## 4. Kết luận

Giải pháp tự động hóa bằng AWS Lambda + CloudWatch + SNS là một sự bổ sung cực kỳ đáng giá để lấp đầy khoảng trống tính năng Auto-scaling trên Amazon RDS Multi-AZ Cluster (readable standbys). Việc tách biệt logic giám sát và tự động xử lý sự cố giúp hệ thống vận hành trơn tru hơn, giảm thiểu rủi ro downtime và giải phóng đáng kể thời gian cho đội ngũ SysAdmin/DBA.

---

## 5. Tài liệu tham khảo

* [AWS Database Blog – Automatically scale storage for Amazon RDS Multi-AZ DB clusters using AWS Lambda](https://aws.amazon.com/blogs/database/automatically-scale-storage-for-amazon-rds-multi-az-db-clusters-using-aws-lambda/)
* [Amazon RDS Multi-AZ deployments documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)
* [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)

<!-- ### 🔗 Bài viết đã đăng trên Facebook Group
👉 **Link bài viết:** [AWS Study Group FB](https://www.facebook.com/groups/awsstudygroupfcj/posts/2224218628343097/) -->