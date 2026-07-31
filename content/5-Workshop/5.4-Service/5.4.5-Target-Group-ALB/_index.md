---
title : "Create Target Groups and Load Balancer"
date : 2026-07-31
weight : 5
chapter : false
pre : " <b> 5.4.5 </b> "
---

We will create Target Groups to serve as the destination endpoints for incoming traffic. After that, we will provision an Application Load Balancer (ALB) to route the traffic and scale the services. The ALB also provides a default DNS name, which we can use for testing later.

#### Create Target Groups
1. Navigate to the **EC2** console, and in the left navigation pane, select **Target Groups**.
2. Choose **Create target group**.
+ Choose a target type: Select **IP addresses**.
+ Target group name: `edushare-tg`
+ Protocol / Port: HTTP
+ Port: 3000 (which is the port for the NestJS backend application).

![Create TG1](/images/5-Workshop/5.4-Service/TG_1.png)

+ VPC: Select **`edushare-vpc`**.
+ Health check path: Enter `/api/v1/health` as this is the existing health check endpoint configured in our backend.
+ **Click Next**. On the Register targets page, do not select anything because there are no Fargate containers running yet, then **Click Create target group**.

![Create TG2](/images/5-Workshop/5.4-Service/TG_2.png)

3. Repeat the exact same process to create another Target Group for the frontend named **`edushare-frontend-tg`**.
+ Both the Backend and Frontend are configured to listen on Port 3000, so you can keep the port setting the same.

#### Create ALB
1. In the left navigation pane of the EC2 console, select **Load balancers** and click **Create load balancer**.
2. The console will direct you to the load balancer type selection page.
+ Under **Application Load Balancer**, click **Create**.

![Create ALB](/images/5-Workshop/5.4-Service/ALB_1.png)

+ Load balancer name: `edushare-alb`
+ Scheme: **Internet-facing**
+ IP address type: **IPv4**
+ VPC: Select `edushare-vpc`

![Create ALB2](/images/5-Workshop/5.4-Service/ALB_2.png)

+ Mappings: Check the first AZ and select the `edushare-public-subnet-a` subnet. Then, check the second AZ and select the `edushare-public-subnet-b-dummy` subnet.
+ Security groups: Select `edushare-alb-sg`.
+ Listeners and routing: Protocol **HTTP** and Port **80**.
+ Default action: Forward to the newly created target group `edushare-tg`.
+ Click **Create load balancer**.

![Create ALB3](/images/5-Workshop/5.4-Service/ALB_3.png)

3. Once the Load Balancer is successfully provisioned, select it and locate the **DNS name** in its details. Copy and save this DNS name so we can use it to test the system later.