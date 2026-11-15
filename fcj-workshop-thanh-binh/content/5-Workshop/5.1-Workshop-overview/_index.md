---
title: "Workshop overview"
date: 2026-11-15
weight: 1
chapter: false
---

NexusOMS uses CloudFront as the public entry point, S3 for the React frontend, and an ALB to route `/api/*` to ECS Fargate tasks in private subnets. Supabase provides PostgreSQL and authentication.

GitHub Actions validates the source, pushes the backend image to ECR, updates the ECS service, builds the frontend, synchronizes it to S3, and creates a CloudFront invalidation.

{{< figure src="/images/5-Workshop/nexusoms/5.1-Workshop-overview/noxus-aws-architect-1.png" title="NexusOMS AWS architecture" >}}

## Deployment flow

1. Prepare the repository, Supabase, and local environment.
2. Create the VPC, subnets, routes, NAT gateways, and security groups.
3. Create the secret, log group, IAM roles, and audit logs.
4. Deploy the backend with ECR, ALB, and ECS Fargate.
5. Deploy the frontend with S3 and CloudFront.
6. Connect GitHub Actions to AWS using OIDC.
7. Verify the application and clean up all resources.
