---
title : "Tạo S3"
date : 2025-07-30
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

Ở phấn này chúng ta sẽ tiến hành tạo S3 đóng vai trò chứa những file được tải lên từ người dùng, và cấu hình CORS cho S3.

![Create S3](/images/5-Workshop/5.4-Service/Create_S3.png)

#### Tạo S3
1. Ở thanh tìm kiếm tìm **"S3"** và nhấn tìm kiếm, sau đó trang trả về giao diện Console của **S3**
2. Nhấn vào **"Create bucket"** để tạo bucket mới
+Ở phần Bucket namespace chọn **Account Regional Namespace**
+ Bucket name: edushare-storage-2026 và vì ở trên ta chọn Bucket namespace theo Region nên tên đúng sẽ có một cuỗi kí tự sau tên ban đầu
+ Ngoài ra không thay đổi default setting gì khác và bấm **Save**

![Create S3](/images/5-Workshop/5.4-Service/Create_S3_2.png)

#### Điều chỉnh CORS cho S3
Bước này là cần thiết vì người dùng có thể Upload file lên từ trình duyệt lên S3, và nếu không cấu hình đúng CORS thì trình duyệt sẽ chặng các kiểu dữ liệu Cross-Origin này.

1. Click vào tên bucket vừa tạo và chuyển sang tab **Permission**
2. Kéo xuống cuối trang tìm mục **Cross-origin resource sharing**
+ Nhấn **Edit**
+ Sau đó cấu hình CORS theo dạng JSON để cho phép các phương thức, và ta cho phép nó đi đến từ tất cả các Origin khác:

```json
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "PUT",
            "POST",
            "DELETE",
            "HEAD"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": [
            "ETag"
        ],
        "MaxAgeSeconds": 3000
    }
]
```

