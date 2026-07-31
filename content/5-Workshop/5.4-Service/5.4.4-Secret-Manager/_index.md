---
title : "Store in AWS Secrets Manager"
date : 2026-07-31
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

AWS Secrets Manager is a service used to securely store sensitive information. Here, we will store the environment variables required by the Backend and Frontend applications. Later, when provisioning the ECS Fargate images, we will retrieve these values directly from Secrets Manager.

#### Create a Secret in Secrets Manager
1. In the search bar, search for **Secrets Manager** and select it to navigate to the **Secrets Manager** console.
2. Click on **Store a new secret**:
- Secret type: Select **Other type of secret**.
- Under Key/value pairs, select the **Plaintext** tab and enter your JSON configuration.

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

- These fields are based on the **`.env`** file of the NestJS Backend.
- To obtain the **DATABASE_URL**, go back to the RDS console, select the database you provisioned, and under the **Connectivity & security** section, you will find the Endpoint URL (e.g., `edushare-db.cyjgquuw273e.us-east-1.rds.amazonaws.com`) and Port (`5432`). `EduShare` is the specific database name created inside it.
- Similarly, to obtain the Cache URL, go to the ElastiCache console, select your Valkey/Redis cluster, and copy its Endpoint.

![Create Cache](/images/5-Workshop/5.4-Service/SM_2.png)

3. Click **Next** and enter the Secret name as **edushare/env-backend-secrets**.
+ Click **Next**.
+ Click **Next** again (leave rotation disabled).
+ Click **Store**.