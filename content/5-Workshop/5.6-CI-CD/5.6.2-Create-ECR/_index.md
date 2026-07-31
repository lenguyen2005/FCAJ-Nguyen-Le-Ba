---
title : "Create Amazon ECR Repositories"
date : 2026-07-30
weight : 2
chapter : false
pre : " <b> 5.6.2 </b> "
---

#### Create ECR Repositories
We need to create two private Elastic Container Registry (ECR) repositories to store the Docker images for our backend and frontend applications.

1. Open the **Amazon ECR Console**.
2. In the left navigation pane, choose **Repositories**, then click **Create repository**.

![ecr-create](/images/5-Workshop/5.6-CI-CD/create_ecr.png)

3. Create the **Backend** Repository:
+ Visibility settings: **Private**
+ Repository name: `edushare-backend`
+ Leave other settings as default and click **Create repository**.

4. Create the **Frontend** Repository:
+ Click **Create repository** again.
+ Visibility settings: **Private**
+ Repository name: `edushare-frontend`
+ Click **Create repository**.

| Repository Name | Visibility | Role |
| :--- | :--- | :--- |
| `edushare-backend` | Private | Stores NestJS API images |
| `edushare-frontend` | Private | Stores Next.js UI images |

5. Note down the **URI** of your repositories (e.g., `123456789012.dkr.ecr.us-east-1.amazonaws.com/edushare-backend`). This will be used in the GitHub Actions workflows.