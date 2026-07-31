---
title : "Create Services"
date : 2026-07-31
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---

Now we will proceed to create a Service within the Cluster, based on the Task Definition to provision, pull the image, and run it. We will create 2 Services: one for the Backend and one for the Frontend. This section will guide you through the Backend setup, and you can follow the same steps for the Frontend.

![Find Cluster](/images/5-Workshop/5.5-ECS/Create_Cluster_2.png)

#### Create ECS Service for Backend
1. In the search bar, search for **ECS** and select it to navigate to the **ECS** console.
2. Locate the previously created cluster, **edushare-cluster**, and click on it.
3. The console will direct you to the cluster's services page. Choose **Create service**.

![Create Service 1](/images/5-Workshop/5.5-ECS/Create_Service_1.png)

+ Family: Select `edushare-api-task` with the latest version **: LATEST**.
+ Service name: `edushare-api-service`
+ Launch type: Select **Fargate**.
+ Desired tasks: Enter `1`.

![Create Service 2](/images/5-Workshop/5.5-ECS/Create_Service_2.png)

4. Scroll down and expand the **Networking** section.
+ VPC: Select `edushare-vpc`.
+ Subnets: Remove the default subnets, and only check **`edushare-private-app-subnet`**.
+ Public IP: **Turn off** (Auto-assign public IP).
+ Security group: Choose **Use an existing security group**, remove the default one, and check **`edushare-ecs-sg`**.

![Create Service 3](/images/5-Workshop/5.5-ECS/Create_Service_3.png)

5. Under the **Load balancing** section:
+ Type: **Application Load Balancer**
+ Load balancer: Select the previously created **`edushare-alb`**.
+ Container to load balance: Select `edushare-app:3000`.
+ Target group: Choose **Use an existing target group** and select **`edushare-tg`**.
+ Click **Create**.

![Create Service 4](/images/5-Workshop/5.5-ECS/Create_Service_4.png)

![Create Service 5](/images/5-Workshop/5.5-ECS/Create_Service_5.png)

6. Wait for the Service to be provisioned and for its targets to show as **"Healthy"**.

![Result](/images/5-Workshop/5.5-ECS/Result.png)

7. Proceed to follow the exact same steps for the Frontend Service. The final result will be 2 Fargate Services running simultaneously.