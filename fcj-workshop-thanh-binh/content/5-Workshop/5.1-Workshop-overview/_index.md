---
title: "Workshop overview"
date: 2026-11-15
weight: 1
chapter: false
---

NexusOMS uses CloudFront as the public entry point, S3 for the React frontend, and an ALB to route `/api/*` to ECS Fargate tasks in private subnets. Each task runs the API and background worker in the same .NET application. The API sends events to SQS FIFO; the worker polls, processes and deletes messages after success. Failed messages move to the dead-letter queue according to the redrive policy.

The built-in Webhook Simulator generates order creation, cancellation and burst scenarios. The demo verifies orders and stock inside NexusOMS; live shop APIs are not integrated. The Open API branch in the diagram represents planned integration.

Supabase provides both PostgreSQL and the authentication already integrated into the application. RDS PostgreSQL replaces only the database; replacing all of Supabase also requires a separate authentication solution.

GitHub Actions sends an OIDC token to AWS STS to assume the IAM role and receive temporary credentials. The workflow uses these credentials to push the backend image to ECR, update the ECS service, build the frontend, synchronize it to S3, and create a CloudFront invalidation.

![NexusOMS business flow architecture](/images/2-Proposal/noxus-aws-architect-1.png)

| Flow | Description |
| --- | --- |
| 5 | The API sends valid webhook events to SQS FIFO. |
| 6 | The worker polls SQS, processes messages and deletes them after success. |
| 10 | The backend connects to Supabase; marketplace Open API access is planned integration. |
| 14 | At startup, Fargate downloads ECR image layers from AWS-managed S3 through the S3 Gateway Endpoint. |

Flows 5–6 apply to both API + Worker replicas in one ECS service. Flow 14 covers the complete Fargate → Endpoint → S3 route. ECR API/registry access still uses NAT Gateways.

To demonstrate both tasks shown in the diagram, set `Desired tasks = 2` and confirm one running task in each AZ. The lab uses `1` task by default.

## Deployment flow

1. Prepare the repository, Supabase, and local environment.
2. Create the VPC, subnets, routes, NAT gateways, and security groups.
3. Create the secret, log group, IAM roles, SQS FIFO queue, DLQ, and audit logs.
4. Deploy the backend with ECR, ALB, and ECS Fargate.
5. Deploy the frontend with S3 and CloudFront.
6. Connect GitHub Actions to AWS using OIDC.
7. Verify orders, cancellation, and the SQS burst demonstration.
8. Clean the demo data and delete all AWS resources.
