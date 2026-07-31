---
title : "Prerequisites"
date : 2026-07-30 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

#### IAM permissions
To successfully deploy and clean up the EduShare workshop resources, your AWS IAM user must have sufficient permissions. In a lab environment, assigning the `AdministratorAccess` managed policy is recommended. Alternatively, you can attach the following custom inline policy covering the exact services used in this project:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EduShareWorkshopPermissions",
            "Effect": "Allow",
            "Action": [
                "ec2:*",
                "ecs:*",
                "ecr:*",
                "elasticloadbalancing:*",
                "rds:*",
                "elasticache:*",
                "s3:*",
                "secretsmanager:*",
                "iam:*",
                "logs:*"
            ],
            "Resource": "*"
        }
    ]
}
```

#### Environment & Region Preparation

In this workshop, we will use **N.Virginia region (us-east-1)** for availability and stability.

This workshop focuses on a manual, step-by-step Cloud-Native deployment via the AWS Management Console to help you deeply understand the architecture. Before proceeding, ensure you have prepared the following:

![create stack](/images/5-Workshop/5.2-Prerequisite/region.png)

+ **AWS Region**: Switch your console region to **us-east-1**.
+ **Source Code**: Have access to the EduShare Frontend (Next.js) and Backend (NestJS) repositories.
+ **GitHub Account**: Required for setting up the CI/CD pipeline using GitHub Actions in later steps.