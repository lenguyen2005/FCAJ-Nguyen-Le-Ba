---
title : "Create Route Tables"
date : 2026-07-30 
weight : 3
chapter : false
pre : " <b> 5.3.3 </b> "
---

#### Create a Public Route Table
We will create a Public Route Table for the Public Subnet to define the routing rules, allowing it to route traffic to the Internet Gateway.

1. In the **VPC** Console, from the left navigation pane, select **Route Tables**.
+ Choose **Create route table**.
+ Name the route table as "edushare-public-rt".
+ Choose **Create route table**.

![Create Route Table](/images/5-Workshop/5.3-VPC-Subnet/Create_RT.png)

2. Return to the Console, select the created **"edushare-public-rt"**, and choose the **Routes** tab at the bottom of the page.
+ Choose **Edit routes** and click **Add route**.
+ Destination: `0.0.0.0/0`
+ Target: select **Internet Gateway** and point it to **"edushare-igw"**.

![Config Route Table](/images/5-Workshop/5.3-VPC-Subnet/Config_RT.png)

3. Return to the Console, choose the **Subnet associations** tab at the bottom of the page, and click **Edit subnet associations**.
+ Check the corresponding Public Subnet, which is **"edushare-public-subnet-a"**.

![Subnet Route Table](/images/5-Workshop/5.3-VPC-Subnet/Subnet_RT.png)

#### Create the remaining Route Tables similarly
Private App Route Table (allows Fargate to route traffic through the NAT Gateway):

- Name: **"edushare-private-app-rt"** with Target set to **"edushare-nat-gw"**, and associate it with the **"edushare-private-app-subnet"**.

Private DB Route Table (a completely isolated network):
- Name: **"edushare-private-db-rt"**. You do not need to add the `0.0.0.0/0` route. Just associate it with the **"edushare-private-db-subnet"**.