---
title: "Week 10 Worklog"
date: 2026-11-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* Complete the asynchronous webhook processing mechanism for NexusOMS using Amazon SQS FIFO and a dead-letter queue.
* Deploy, test, and troubleshoot the updated version on AWS ECS Fargate.
* Complete the Workshop, Proposal, architecture diagram, and program reports.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Designed an asynchronous webhook flow using Amazon SQS FIFO and a dead-letter queue. <br> - Added a local execution mode for environments without AWS SQS. <br> - Implemented order creation, order cancellation, and inventory restoration from webhooks. <br> - Built a webhook simulator for sending batches and monitoring processing progress. <br> - Added controls to end the demo, remove test data, and load sample data into Supabase. <br> - Synchronized the source code across repositories, including a clean repository for workshop participants. | 16/11/2026 | 16/11/2026 | <https://github.com/sneakez/nexus-aws> |
| 3 | - Deployed and tested SQS with the backend on ECS Fargate. <br> - Fixed webhook timeouts, messages sent to the DLQ, and empty SQS responses. <br> - Adjusted demo data to prevent incorrect inventory changes. <br> - Fixed the order detail view and invalid boolean data. <br> - Retested order creation, cancellation, and load-test flows. <br> - Added SQS, IAM policy, environment variable, and cleanup instructions to the workshop. <br> - Updated the Proposal and architecture diagram with the asynchronous processing flow. | 17/11/2026 | 17/11/2026 | <https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html> |
| 4 | - Reviewed the workshop against the actual AWS deployment process. <br> - Standardized repository preparation, GitHub Actions, OIDC, and CI/CD instructions. <br> - Converted images to Markdown syntax for stable display and easier replacement. <br> - Standardized PowerShell commands so participants could copy and run them directly. <br> - Reordered instructions and screenshots to match the execution sequence. | 18/11/2026 | 18/11/2026 | <https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/about-security-hardening-with-openid-connect> |
| 5 | - Standardized the IAM role, Secrets Manager, and ECR repository instructions. <br> - Updated the Application Load Balancer, ECS cluster, Task Definition, and ECS Service sections for the latest AWS console interface. <br> - Completed the S3 frontend, CloudFront Distribution, and CloudFront Function instructions for the React SPA. <br> - Fixed JSON, JavaScript, and PowerShell formatting issues in the workshop. <br> - Reviewed the CloudTrail, CloudWatch, and S3 access-log flow. | 19/11/2026 | 19/11/2026 | <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html> |
| 6 | - Added repository permission checks and GitHub Actions OIDC configuration steps. <br> - Clarified deployment, verification, and common troubleshooting steps. <br> - Added CloudWatch Alarm and resource cleanup instructions. <br> - Updated the primary clone source to the `sneakez/nexus-aws` repository. <br> - Reviewed the Proposal, monthly operating costs, and the cost-saving option. <br> - Checked the Vietnamese and English content and verified the website build. | 20/11/2026 | 20/11/2026 | <https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html> |
| 7 | - Attended AWS Student Community Day Vietnam 2026 at the AWS office. <br> - Wrote and revised the event report in Vietnamese and English. <br> - Completed the Proposal with WAF and two NAT Gateway costs, together with a cost-saving option using one NAT Gateway without WAF. <br> - Completed the Self-assessment and Sharing & Feedback sections. <br> - Added the Vietnamese version of the Event 3 report. <br> - Reviewed personal information, template content, and placeholders across the website. <br> - Verified wording, navigation links, and the website build. | 21/11/2026 | 21/11/2026 |  |


### Week 10 Achievements:

* Completed asynchronous webhook processing using Amazon SQS FIFO and a dead-letter queue.
* Completed the order creation, cancellation, load-test, and demo-data cleanup flows.
* Deployed and tested the updated version on ECS Fargate.
* Synchronized the source code and prepared a clean repository for workshop participants.
* Completed most workshop instructions based on the AWS console interface used during practice.
* Updated the Proposal, architecture diagram, and operating cost estimate.
* Completed the event report, Self-assessment, and Sharing & Feedback sections.
