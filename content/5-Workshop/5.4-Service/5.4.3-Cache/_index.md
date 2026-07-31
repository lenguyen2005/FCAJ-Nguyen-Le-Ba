---
title : "Create ValkeyCache"
date : 2026-07-31
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

Amazon ElastiCache is an in-memory data store service used as a cache and real-time database to accelerate application performance, alleviate the load on primary databases, and support session management and message queuing. In this section, we will provision a cache using the new Valkey engine, which is recommended by AWS.

#### Create ValkeyCache
1. In the search bar, search for **ElastiCache** and select it to navigate to the **ElastiCache** console.

![Create Cache](/images/5-Workshop/5.4-Service/Create_Cache.png)

2. In the left navigation pane, select **Valkey caches** and choose **Create cache**.
- Under Cache settings:
+ Engine: Select **Valkey**
+ Deployment option: Select **Node-based cluster**
+ Creation method: Select **Easy create**

- Under Configuration:
+ Select **Demo**

![Config Cache 1](/images/5-Workshop/5.4-Service/Config_Cache_1.png)

- Under Cluster info:
+ Name: edushare-redis

![Config Cache 2](/images/5-Workshop/5.4-Service/Config_Cache_2.png)

- Click **Create** at the bottom to provision the cache.