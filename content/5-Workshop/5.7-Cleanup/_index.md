---
title : "Clean up resources"
date : 2026-07-31
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

#### Clean up Cluster
1. Access the **ECS Console**.
2. Select the cluster **`edushare-cluster`**.
3. In the **Services** tab, select the services and click **Delete**.

![Cluster Clean up](/images/5-Workshop/5.7-Cleanup/Cleanup_Cluster.png)

#### Clean up Load Balancers and Target Groups
1. From the left navigation pane, go to **Load Balancers**, select **`edushare-alb`**, click **Actions**, and select **Delete load balancer**.

![Load balancers Clean up](/images/5-Workshop/5.7-Cleanup/LB_Delete.png)

2. From the left navigation pane, go to **Target Groups**, select **`edushare-tg`** and **`edushare-frontend-tg`**, click **Actions**, and select **Delete**.

![Definition Clean up](/images/5-Workshop/5.7-Cleanup/Dereg_Task_Def.png)

#### Delete Database and Cache
1. Access the **RDS Console** and go to **Databases**. Select **`edushare-db`**, click **Actions**, and choose **Delete**.

![RDS Clean up](/images/5-Workshop/5.7-Cleanup/RDS_Delete.png)

2. **ElastiCache Valkey:** Access the **ElastiCache Console** -> **Valkey caches**. Select **`edushare-redis`**, click **Delete**, and choose not to create a backup if prompted.

![Cache Clean up](/images/5-Workshop/5.7-Cleanup/Cache_Delete.png)

#### Delete NAT Gateway and Elastic IP
Access the **VPC Console** -> **NAT Gateways**. Select **`edushare-nat-gw`**, click **Actions** -> **Delete NAT gateway**. Type `delete` to confirm.

![NAT Clean up](/images/5-Workshop/5.7-Cleanup/NAT_Delete.png)

#### Delete S3 and Secrets Manager
1. Access the **S3 Console**.
2. Select the bucket, click the **Empty** button first, and type `delete` to remove all objects inside.
3. Once emptied successfully, select the bucket again, click **Delete**, and type the bucket name to confirm deletion.
4. Access **Secrets Manager**, select **`edushare/env-backend-secrets`**, click **Actions**, and choose **Delete secret**.

![S3 Clean up](/images/5-Workshop/5.7-Cleanup/S3_Delete.png)

![Secret Manager Clean up](/images/5-Workshop/5.7-Cleanup/NAT_Delete.png)

#### Delete VPC, Subnets, and Security Groups
Access the **VPC Console**, go to **Your VPCs**, select **`edushare-vpc`**, and click **Actions** -> **Delete VPC**.

![VPC Clean up](/images/5-Workshop/5.7-Cleanup/VPC_Delete.png)

#### Delete IAM Roles and CloudWatch
1. Access the **IAM Console** and go to **Roles**. Search for and delete the two roles: **`edushare-ecs-execution-role`** and **`edushare-ecs-task-role`**.

![IAM Clean up](/images/5-Workshop/5.7-Cleanup/IAM_Delete.png)

2. Access the **CloudWatch Console** and go to **Log groups**. Search for and delete the log group **`/aws/ecs/edushare-api`** (and the frontend log group if applicable).

![CloudWatch Clean up](/images/5-Workshop/5.7-Cleanup/CloudWatch_Delete.png)