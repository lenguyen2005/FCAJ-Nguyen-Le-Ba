---
title : "Lưu AWS Secrets Manager"
date : 2026-07-31
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

Secrets Manager là nơi để lưu các thông tin để đảm bảo tính bảo mật. Ở đây, ta sẽ lưu các trường mà trong file Backend và Frontend yêu cầu để đọc. Sau này khi khởi tạo chạy các Image ECS thì ta sẽ lấy các giá trị này từ Secret Manager.

#### Tạo Secret Manager
1. Tại thanh tìm kiếm, tìm **"Secret Manager"** và nhấn chọn, sẽ dẫn đến Console của **"Secret Manager"**
2. Chọn **"Store a new secret"**:
- Chọn Secret Type: Ohter types
- Key và Value Pair chọn Plain Text và nhập:

```json
{
  "DATABASE_URL": "postgresql://<master user>:<pass>@edushare-db.cyjgquuw273e.us-east-1.rds.amazonaws.com:5432/EduShare?schema=public&sslmode=no-verify",
  "REDIS_HOST": "edushare-redis.cyxlqm.ng.0001.use1.cache.amazonaws.com",
  "REDIS_PORT": "6379",
  "JWT_SECRET": "<your JWT Secret Key>",
  "S3_REGION": "us-east-1",
  "S3_BUCKET_NAME": "edushare-storage-2026-652892608089-us-east-1-an"
}
```

![Create Cache](/images/5-Workshop/5.4-Service/SM_1.png)

- Các trường này dựa trên **".env"** của Netxt.js Backend.
- Để lấy được **DATABASE_URL** thì quay lại Mục Database và chọn và Database đã khởi tạo, ở phía dưới **Information** sẽ hiện ra URL của Database là phần ```edushare-db.cyjgquuw273e.us-east-1.rds.amazonaws.com:5432```. Còn ```EduShare``` là Database đã tạo trong đó từ trước. 
- Tương tự, để lấy đuọc URL của Cache thì vào Elastic Cache đã tạo và lấy URL của nó. 

![Create Cache](/images/5-Workshop/5.4-Service/SM_2.png)

3. Bấm **Next** và nhập tên là **edushare/env-backend-secrets**.
+ Bấm **Next**
+ Bấm **Next**
+ Click **Store**