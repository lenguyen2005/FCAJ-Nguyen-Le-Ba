---
title : "Core Compute"
date : 2026-07-31
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

#### Overview
In this section, we will deploy the core compute layer of the EduShare system using **Amazon Elastic Container Service (ECS)** powered by **AWS Fargate** (a serverless compute engine for containers). 

Instead of managing traditional virtual machines (EC2), we will focus entirely on packaging and running our application codes. We will define how our containers should run using **Task Definitions**, group our computational resources logically into an **ECS Cluster**, and finally launch our Backend and Frontend applications as highly available **ECS Services** integrated with our Application Load Balancer (ALB).


#### Content

- [1. Create ECS Cluster and Task Definition](1-create-cluster-task-def/)
- [2. Create Services](2-create-services/)