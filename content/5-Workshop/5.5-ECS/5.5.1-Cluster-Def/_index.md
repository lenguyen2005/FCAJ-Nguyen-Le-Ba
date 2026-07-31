---
title : "Create ECS Cluster and Task Definition"
date : 2025-07-30
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---

In this section, we will proceed to create an ECS Cluster and Task Definitions. A Task Definition is a detailed blueprint or configuration file that specifies how one or more containers will run, including the Docker image, CPU, memory, and ports. An ECS Cluster is a logical grouping of resources or servers, such as EC2 or Fargate, used to deploy and run those tasks.

#### Create ECS Cluster
1. In the search bar, search for **Elastic Container Service** and select it to navigate to the ECS console.
2. Choose **Create cluster**.
+ Cluster name: `edushare-cluster`
+ Infrastructure: AWS will have **AWS Fargate (serverless)** checked by default.
+ Click **Create**.

![Create Cluster](/images/5-Workshop/5.5-ECS/Create_Cluster_1.png)

#### Define Task Definitions
We will define Task Definitions for both the Backend and Frontend. We will start with the Backend first, and you can follow the same process for the Frontend.
1. In the left navigation pane, select **Task definitions**.
2. Choose **Create new task definition**.
+ Task definition family: `edushare-api-task`
+ Launch type: Select **AWS Fargate**
+ OS, Architecture: **Linux/X86_64**

![Create Task Manager 1](/images/5-Workshop/5.5-ECS/Task_Def_1.png)

+ CPU: **.5 vCPU**
+ Memory: **1 GB**
+ Task role: Select **edushare-ecs-task-role** (this grants the NestJS app permissions to interact with S3).
+ Task execution role: Select **edushare-ecs-execution-role** (this grants AWS permissions to pull images and fetch secrets).

![Find ECR](/images/5-Workshop/5.5-ECS/ECR.png)

3. Open a new AWS tab, search for **ECR (Elastic Container Registry)** in the search bar to navigate to the registry containing our pre-built Backend and Frontend images.
+ Copy the Backend's Image URI.

4. Return to the Task Definition setup tab.
- Under **Container - 1**:
+ Name: `edushare-app`
+ Image URI: Paste the ECR Image URI string here.
+ Container port: `3000`
+ Protocol: **TCP**

![Create Task Manager 2](/images/5-Workshop/5.5-ECS/Task_Def_2.png)

- **Environment variables**:
+ Click **Add environment variable**.
+ We will enter the exact keys we saved in Secrets Manager earlier.
+ Select the **ValueFrom** type.
+ For the Value, go back to your Secrets Manager tab, copy the ARN of the secret you created.
+ For example, if the Key is **DATABASE_URL**, the Value will be `<Secret Manager ARN>:DATABASE_URL::`.

![Create Task Manager 3](/images/5-Workshop/5.5-ECS/Task_Def_3.png)

5. Click **Create**.
6. Repeat the exact same process to create a Task Definition for the Frontend named **edushare-frontend-task**.
+ Note: Remember to copy and use the Frontend's Image URI from the ECR page for this task.