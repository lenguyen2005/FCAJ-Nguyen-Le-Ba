---
title : "Create VPC and Subnets"
date : 2026-07-30 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

#### Create VPC
1. Open the [Amazon VPC console](https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1#Home:)
2. In the search bar, search for **VPC**, select **VPC**, and navigate to the VPC dashboard.

![VPC](/images/5-Workshop/5.3-VPC-Subnet/Create_VPC.png)

3. In the **VPC** Console, choose **Create VPC**:
+ Select **VPC only**
+ Name the **VPC** as "edushare-vpc"
+ IPv4 CIDR block: Select **Manual input** and enter `10.0.0.0/16`, then click **Create VPC**

![VPC2](/images/5-Workshop/5.3-VPC-Subnet/Create_VPC_2.png)

+ After creating the **VPC**, select the **"edushare-vpc"** again and click on **Actions** to **Edit VPC**
+ Check the **"DNS Setting"** boxes to enable **"Enable DNS hostnames"** and **"Enable DNS resolution"** so that RDS and ECR can resolve internal domain names.

![VPCDNS](/images/5-Workshop/5.3-VPC-Subnet/Create_VPC_DNS.png)

#### Create Subnets
1. Return to the VPC dashboard, and in the left navigation pane, choose **Subnets**
+ Click **Create subnet**

| Subnet Name (Name tag) | Availability Zone (AZ) | IPv4 CIDR block | Role |
| :--- | :--- | :--- | :--- |
| `edushare-public-subnet-a` | Choose the first AZ (e.g., `us-east-1a`) | `10.0.1.0/24` | Hosts NAT Gateway, ALB |
| `edushare-public-subnet-b-dummy` | Choose the second AZ (e.g., `us-east-1b`) | `10.0.2.0/24` | Dummy subnet to satisfy ALB Multi-AZ requirements |
| `edushare-private-app-subnet` | Choose the first AZ (`us-east-1a`) | `10.0.10.0/24` | Hosts ECS Fargate (NestJS) |
| `edushare-private-db-subnet` | Choose the first AZ (`us-east-1a`) | `10.0.20.0/24` | Hosts RDS PostgreSQL & Redis |

![endpoint](/images/5-Workshop/5.3-VPC-Subnet/Create_Subnet.png)

2. In the Create Subnet section, fill in the following information:
+ Select **VPC ID** as **"edushare-vpc"**
+ Subnet name: **"edushare-public-subnet-a"**
+ Choose **AZ** as **"us-east-1a"**
+ IPv4 CIDR block: `10.0.1.0/24`

3. Repeat the same process for the remaining Subnets.