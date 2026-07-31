---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Automatically scaling storage for Amazon RDS Multi-AZ DB Cluster with AWS Lambda

When managing and operating database systems on AWS, one of the most critical incidents that DBAs or DevOps encounter is when RDS runs out of storage space. When the disk reaches 100% capacity, the database immediately goes into an Inaccessible-storage state, disrupting the entire application, breaking pending transactions, and requiring a significant amount of time to recover.

Although the built-in Storage Auto-scaling feature of Amazon RDS works very well on Single-AZ or standard Multi-AZ instances, Amazon RDS Multi-AZ DB clusters do not yet support this automated feature out of the box. Having to monitor manually, then go to the console to send a modify instance command, and waiting from several tens of minutes to a few hours for it to apply not only creates operational pressure but is also very risky if an incident occurs outside of working hours.

Therefore, I researched a lightweight but highly effective automation solution: combining Amazon CloudWatch, AWS Lambda, and Amazon SNS to build an automated mechanism to scale up disk capacity for the RDS Multi-AZ Cluster as soon as it hits the warning threshold.

---

## 1. Architecture Overview and Implementation Steps

The system is designed following an event-driven architecture, ensuring proactiveness, immediate response, and fully utilizing native AWS services without needing to install third-party tools.

![Architecture Overview](/images/3-Blog/3.2-Blog2/blog2.jpg)

### A. Monitoring and Detection Flow
* **Step 1:** Set up a CloudWatch Alarm to closely monitor the `FreeStorageSpace` metric of each RDS DB Instance within the Cluster.
* **Step 2:** Configure a safe trigger threshold. The recommended level is usually when the remaining free disk space falls below 15% of the total initial capacity (e.g., if the drive is 100 GB, an alarm will trigger when 15 GB of free space remains).

### B. Processing & Auto-scaling Flow
* **Step 3:** As soon as the disk hits the threshold, the CloudWatch Alarm transitions to the ALARM state and automatically triggers an AWS Lambda function via a Resource-based Policy.
* **Step 4:** The Lambda function receives the event information and queries the AWS SDK (Boto3) directly to retrieve the current configuration details of the DB Cluster (such as `AllocatedStorage`, drive type gp3, io1, io2, or IOPS/Throughput parameters).
* **Step 5:** Lambda calculates the new capacity based on the `SCALING_PERCENTAGE` parameter configured via an environment variable (by default, it increases by 15% to optimize costs, or 30%–40% if the system has an extremely fast data write rate).
* **Step 6:** Lambda executes the `modify_db_cluster` function with the `ApplyImmediately = True` option to upgrade the disk size immediately, while automatically preserving the accompanying performance parameters.

### C. Notification Flow
* **Step 7:** Simultaneously with invoking Lambda, the CloudWatch Alarm sends an alert message via Amazon SNS to the operations team's Email or Slack channel so everyone is aware of the ongoing automated disk expansion event.

---

## 2. A Few Technical Highlights Found Very Useful

* **Smart Handling of Disk Types (IOPS & Throughput):** The highlight of the Lambda code in this solution is its ability to recognize the storage disk type. If you use a gp3 disk, Lambda will maintain the baseline IOPS (3000) and Throughput. If you use a Provisioned IOPS disk (io1/io2), Lambda will automatically recalculate and pass the mandatory AWS IOPS parameters when changing the disk size, preventing the request from failing due to missing parameters.
* **Fully Automated Without Manual Intervention:** There is no longer a scenario where a technician has to receive an alert at 2 AM, open the computer to type commands in the console, and wait. The system automatically detects and executes the expansion within seconds.
* **Large-scale Deployment using CloudFormation:** The article provides a CloudFormation template allowing you to manage tens or hundreds of DB Instances simultaneously simply by passing a list of IDs and corresponding thresholds (`prod-db-1`, `prod-db-2`). This is very convenient for Enterprise environments with numerous database resources.

---

## 3. Some Important Notes Before Implementation

* **AWS RDS Cooldown Period Rules:** Amazon RDS strictly requires a minimum cooldown period of 6 hours between two storage capacity modifications on the same DB. This means if you only scale up by an additional 10% but the data expands too rapidly within the following 1-2 hours, Lambda will not be able to scale a second time. Therefore, choose a sufficiently safe increment percentage, around 20%–30%.
* **Storage Costs Only Increase, Never Decrease:** The disk capacity on RDS cannot be shrunk once it has been upgraded. All storage costs will be billed according to the new capacity permanently. You need to carefully calculate the increment level to avoid wasting budget.
* **Maximum Limits of the Database Engine:** Each DB Engine type and Instance class has a maximum disk limit, for example, 64 TB or 128 TB. This solution currently does not include code to check the maximum capacity ceiling; you should be mindful of this limit.

---

## 4. Conclusion

The automation solution using AWS Lambda + CloudWatch + SNS is an extremely valuable addition to fill the Auto-scaling feature gap on Amazon RDS Multi-AZ Clusters (readable standbys). Decoupling the monitoring logic from the automated incident response makes the system run smoother, minimizes downtime risks, and frees up significant time for the SysAdmin/DBA team.

---

## 5. References

* [AWS Database Blog – Automatically scale storage for Amazon RDS Multi-AZ DB clusters using AWS Lambda](https://aws.amazon.com/blogs/database/automatically-scale-storage-for-amazon-rds-multi-az-db-clusters-using-aws-lambda/)
* [Amazon RDS Multi-AZ deployments documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)
* [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)