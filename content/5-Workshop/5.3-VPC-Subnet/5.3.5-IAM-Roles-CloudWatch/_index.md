---
title : "Create IAM Roles and CloudWatch"
date : 2026-07-30 
weight : 5
chapter : false
pre : " <b> 5.3.5 </b> "
---

In this section, we will create IAM Roles for ECS Fargate to grant it permissions to pull images and write logs, as well as allow the NestJS application code to interact with S3.

In the search bar, search for and select **IAM**, then from the left navigation pane, choose **Roles**.

![Create IAM](/images/5-Workshop/5.3-VPC-Subnet/IAM_Create.png)

#### Create Task Execution Role
We create permissions for ECS to pull images and write logs.
1. Choose **Create role**.
2. In Step 1:
+ Trusted entity type: Select **AWS service**.
+ Use case: Select **Elastic Container Service**, and then select **Elastic Container Service Task**.

3. In Step 2:
+ In the permissions search box, type and check the policy named: `AmazonECSTaskExecutionRolePolicy`.
+ Find and also check the policy: `SecretsManagerReadWrite` so Fargate can read environment variables from Secrets Manager.

4. In Step 3:
+ Role name: `edushare-ecs-execution-role`.

![Create IAM Permission](/images/5-Workshop/5.3-VPC-Subnet/IAM_Per.png)

#### Create Task Role
Create permissions for the NestJS code to interact with S3.
1. Choose **Create role**.
2. In Step 1:
+ Trusted entity type: Select **AWS service**.
+ Use case: Select **Elastic Container Service**, and then select **Elastic Container Service Task**.

3. In Step 2:
+ In the permissions search box, type and check the policy named `AmazonS3FullAccess` to allow the NestJS app to generate Presigned URLs for users to upload documents to S3.

4. In Step 3:
+ Role name: `edushare-ecs-task-role`.

#### Create CloudWatch Log Group
1. Switch to the **CloudWatch** service using the search bar.
2. In the left navigation pane under **Log management** (or **Logs**), select **Log groups** and choose **Create log group**.
+ Log group name: `/aws/ecs/edushare-api`
+ Retention setting: Select **7 days** to avoid storing logs for too long, which incurs unnecessary costs.

![Create CloudWatch](/images/5-Workshop/5.3-VPC-Subnet/CW.png)