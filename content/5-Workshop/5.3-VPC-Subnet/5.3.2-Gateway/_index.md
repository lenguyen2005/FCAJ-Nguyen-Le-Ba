---
title : "Create Internet Gateway and NAT Gateway"
date : 2026-07-30 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Create Internet Gateway
1. In the **VPC** Console, from the left navigation pane, select **Internet Gateways**.
+ Choose **Create internet gateway**.
+ Enter the Name tag as "edushare-igw".
+ Choose **Create internet gateway** to create the Internet Gateway.

![Create IG](/images/5-Workshop/5.3-VPC-Subnet/Create_IG.png)

2. Return to the **Internet Gateways** section in the **VPC** Console, select **"edushare-igw"**, and click **Actions**.
+ Choose **Attach to VPC** and select the previously created VPC, **"edushare-vpc"**.

![Attach IG](/images/5-Workshop/5.3-VPC-Subnet/Attach_VPC_IG.png)

#### Create NAT Gateway

1. In the **VPC** Console, from the left navigation pane, select **NAT Gateways**.
2. Choose **Create NAT gateway**.
+ **Name tag**: enter "edushare-nat-gw".
+ **Subnet**: select **"edushare-public-subnet-a"**.
+ **Connectivity type**: select "Public".
+ **Elastic IP allocation ID**: click the **Allocate Elastic IP** button so AWS will automatically assign a static IP to this NAT Gateway.
+ **Click Create NAT gateway**.

![Create NAT](/images/5-Workshop/5.3-VPC-Subnet/Create_NAT.png)