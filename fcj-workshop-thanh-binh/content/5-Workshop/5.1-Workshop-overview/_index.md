---
title: "Workshop overview"
date: 2026-11-15
weight: 1
chapter: false
---

NexusOMS uses CloudFront as the public entry point, S3 for the React frontend, and an ALB to route `/api/*` to ECS Fargate tasks in private subnets. The API sends webhooks to an SQS FIFO queue and an ECS worker processes each message. Messages that fail more than three times move to a dead-letter queue. Supabase provides PostgreSQL and authentication.

GitHub Actions sends an OIDC token to AWS STS to assume the IAM role and receive temporary credentials. The workflow uses these credentials to push the backend image to ECR, update the ECS service, build the frontend, synchronize it to S3, and create a CloudFront invalidation.

![NexusOMS business flow architecture](/images/2-Proposal/noxus-aws-architect-1.png)

## Deployment flow

1. Prepare the repository, Supabase, and local environment.
2. Create the VPC, subnets, routes, NAT gateways, and security groups.
3. Create the secret, log group, IAM roles, SQS FIFO queue, DLQ, and audit logs.
4. Deploy the backend with ECR, ALB, and ECS Fargate.
5. Deploy the frontend with S3 and CloudFront.
6. Connect GitHub Actions to AWS using OIDC.
7. Verify orders, cancellation, and the SQS burst demonstration.
8. Clean the demo data and delete all AWS resources.
