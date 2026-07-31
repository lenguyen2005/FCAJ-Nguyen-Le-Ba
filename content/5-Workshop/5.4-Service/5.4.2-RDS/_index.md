---
title : "Provision Database"
date : 2026-07-31
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

After setting up S3, we need a PostgreSQL Database to store other lightweight relational data. For example, user login credentials, or the Keys and Values of files stored in S3 in case we need to query and preview them. We will first create a DB subnet group to define which subnets the database can operate in.

#### Create DB Subnet Group
1. In the search bar, search for **RDS** and select it to navigate to the **RDS** console.
2. In the left navigation pane, select **Subnet groups** and click **Create DB subnet group**.

![Create Subnet Group](/images/5-Workshop/5.4-Service/DB_Subnet.png)

3. In the create subnet group interface:
+ Name: `edushare-db-subnet-group`
+ VPC: Choose `edushare-vpc`
+ Under **Add subnets**: choose 2 Availability Zones (e.g., `1a` and `1b`).
+ Choose subnets: Check the box for **`edushare-private-db-subnet`** which corresponds to one of the selected AZs.
+ Click **Create**.

![Config Subnet Group](/images/5-Workshop/5.4-Service/DB_Subnet_Config.png)

#### Provision RDS Instance
1. In the left navigation pane, select **Databases** and choose **Create database**.
2. In the database creation interface:
- Engine options: select **Aurora (PostgreSQL-Compatible Edition)**.
- Database management type / Create method: **Standard create**.
- Templates: **Dev/Test**.
- Cluster scalability type: **Provisioned** (to select available instance classes).

![Create RDS 1](/images/5-Workshop/5.4-Service/Create_DB_1.png)

- Select the **db.t3.medium** instance class to save costs.
- Under **Settings**:
+ DB instance identifier: `edushare-db`
+ Set your desired **Master username**.
+ Credential Settings: Choose **Self managed** and enter a password for the **Master password**. Be sure to save both the username and password securely.
- **Storage**: Select General Purpose SSD (gp2/gp3) and Allocated storage: 20 GB. Additionally, uncheck **Enable storage autoscaling** to avoid unexpected costs.

![Create RDS 1](/images/5-Workshop/5.4-Service/Create_DB_2.png)

- Under **Connectivity**:
+ Virtual private cloud (VPC): Select `edushare-vpc`.
+ DB subnet group: Select the newly created `edushare-db-subnet-group`.
+ Public access: Select **No** to isolate the DB from the public Internet.
+ VPC security group (firewall): Choose **Choose existing** and select `edushare-db-sg`.

![Create RDS 1](/images/5-Workshop/5.4-Service/Create_DB_3.png)

3. Click **Create database** and wait for the provisioning to complete.
+ After about 1 to 5 minutes, the database will be successfully created and its status will change to **Available**.