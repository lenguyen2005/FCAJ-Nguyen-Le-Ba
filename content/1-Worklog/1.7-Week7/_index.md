---
title: "Worklog Week 7"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

- Set up the outer security layer: Amazon CloudFront, AWS WAF, and Amazon Route 53
- Integrate Amazon S3 for document and user avatar storage

### Tasks to be implemented this week:

| Day | Task | Start Date | Completion Date | Reference Materials |
| --- | ---- | ---------- | --------------- | ------------------- |
| Mon | - Create CloudFront Distribution pointing to ALB <br> - Configure Origin and Cache Behavior | 07/20/2026 | 07/20/2026 | <https://docs.aws.amazon.com/cloudfront/> |
| Tue | - Set up AWS WAF Web ACL <br> - Attach WAF to the CloudFront Distribution | 07/21/2026 | 07/21/2026 | <https://docs.aws.amazon.com/waf/> |
| Wed | - Configure Amazon Route 53 domain <br> - Create SSL Certificate using ACM | 07/22/2026 | 07/22/2026 | <https://docs.aws.amazon.com/route53/> |
| Thu | - Integrate Amazon S3 for storing Docs, Images, Avatars <br> - Configure Presigned URLs for secure uploads | 07/23/2026 | 07/23/2026 | <https://docs.aws.amazon.com/s3/> |
| Fri | - End-to-End system testing <br> - Set up CloudWatch Logs and Alarms | 07/24/2026 | 07/24/2026 | |

### Achievements in Week 7:

- Successfully set up **Amazon CloudFront** as the CDN and single entry point for all traffic:
  - Origin pointing to the ALB.
  - Separate Cache Behaviors for API and static assets.

- Integrated **AWS WAF** to protect the application from common attacks:
  - AWS Managed Rules: Core Rule Set, Known Bad Inputs.
  - Rate-based rule for basic DDoS protection.

- Configured **Amazon Route 53** with a custom domain and SSL/TLS Certificate from ACM.

- Integrated **Amazon S3** for document and image storage:
  - Separate buckets for Docs, Images, and Avatars.
  - Used **Presigned URLs** to allow users to upload files directly to S3, reducing load on ECS.
  - Bucket Policy restricts read access to CloudFront only for static assets.

- Set up **Amazon CloudWatch**:
  - Log Group for ECS Tasks.
  - Metric Alarms for CPU, Memory, and ALB 5xx errors.

- Completed the overall architecture and performed successful end-to-end testing.
