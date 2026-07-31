---
title : "IAM User & GitHub Secrets"
date : 2026-07-30
weight : 1
chapter : false
pre : " <b> 5.6.1 </b> "
---

#### Create an IAM User for GitHub Actions

To allow GitHub Actions to securely push Docker images to Amazon ECR and update Amazon ECS services, we need to create a dedicated IAM User with access through an Access Key.

1. Open the **Amazon IAM Console**.
2. In the left navigation pane, select **IAM Users**, then click **Create user**.

   * **User name:** `github-actions-deployer`
   * **Do not** select **Provide user access to the AWS Management Console**, then click **Next**.

![iam-user](/images/5-Workshop/5.6-CI-CD/create_iam_user.png)

3. On the **Set permissions** page, select **Attach policies directly**.

   * Search for and select the following Managed Policies:

     * `AmazonEC2ContainerRegistryPowerUser` (allows pushing Docker images to Amazon ECR)
       ![ECR-permission](/images/5-Workshop/5.6-CI-CD/ECR_permission.png)
     * `AmazonECS_FullAccess` (allows triggering new deployments on Amazon ECS)
       ![ECS-permission](/images/5-Workshop/5.6-CI-CD/ECS_permission.png)
   * Click **Next**, then select **Create user**.

   ![create](/images/5-Workshop/5.6-CI-CD/create_user.png)

4. Create an Access Key:

   * Select the newly created IAM User `github-actions-deployer`.
   * Go to the **Security credentials** tab.
   * Under **Access keys**, click **Create access key**.
   * Select **Third-party service**, check the confirmation box, then click **Next**.
     ![create](/images/5-Workshop/5.6-CI-CD/create_access_key.png)
   * Then click **Create Access Key**.
   * Copy the **Access key ID** and **Secret access key**, and store them in a safe place.

#### Configure GitHub Secrets

1. Open your project's GitHub repository.
2. Navigate to **Settings** → **Secrets and variables** → **Actions**.
3. Click **New repository secret** and add the following secrets:

   * `AWS_ACCESS_KEY_ID`: Paste the **Access Key ID** you created.
   * `AWS_SECRET_ACCESS_KEY`: Paste the corresponding **Secret Access Key**.
   * `NEXT_PUBLIC_API_URL`: The DNS address of the **Application Load Balancer (ALB)** (for example, `https://api.edushare.com`) used during the Frontend build process.

![github-secrets](/images/5-Workshop/5.6-CI-CD/github_secrets.png)
