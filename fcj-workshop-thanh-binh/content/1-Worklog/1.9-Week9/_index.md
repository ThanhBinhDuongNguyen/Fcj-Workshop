---
title: "Week 9 Worklog"
date: 2026-11-09
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* Verify the backend container and prepare the AWS deployment.
* Deploy NexusOMS to AWS (backend on ECS Fargate, frontend on S3 and CloudFront) and fix the deployment errors.
* Set up CI/CD with GitHub Actions and AWS OIDC.
* Complete the bilingual workshop documentation and the high-availability architecture diagram.

### Tasks to be carried out this week:

| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Start Date | Completion Date | Reference Material                                                          |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | --------------------------------------------------------------------------- |
| 1   | - Fix the backend Docker image by adding the Kerberos library for Npgsql <br> - Check that the backend container can connect to PostgreSQL/Supabase                                                                                                                                                                                                                                                                                                                                                                       | 09/11/2026 | 09/11/2026      | NexusOMS repository (GitHub)                                                |
| 2   | - Review the AWS deployment architecture, the estimated cost and the frontend/backend deployment flow <br> - Prepare resources and the demo guide                                                                                                                                                                                                                                                                                                                                                                         | 10/11/2026 | 10/11/2026      | AWS Management Console                                                      |
| 3   | - Update frontend dependencies <br> - Continue the Proposal and the AWS demo plan                                                                                                                                                                                                                                                                                                                                                                                                                                         | 11/11/2026 | 11/11/2026      | NexusOMS repository (GitHub) <br> NexusOMS proposal                         |
| 4   | - Deploy NexusOMS to AWS and fix the backend HTTP 500 error <br> - Fix the frontend session timeout <br> - Fix product selection by stock and add the initial stock quantity when creating a product <br> - Sync and clean up the repositories, then push the changes to the GitHub repositories                                                                                                                                                                                                                          | 12/11/2026 | 12/11/2026      | AWS Management Console <br> NexusOMS repository (GitHub)                    |
| 5   | - Practice AWS infrastructure: VPC, two public subnets, two private subnets, Internet Gateway, two NAT Gateways, route tables, security groups, ALB, target group, ECS and Fargate <br> - Fix errors with subnets, the IGW route, security groups, Secrets Manager, the task role and health checks <br> - Clean up resources after failed deployments                                                                                                                                                                    | 13/11/2026 | 13/11/2026      | AWS Management Console                                                      |
| 6   | - Complete the bilingual NexusOMS workshop documentation <br> - Normalize image file names so they work on Vercel <br> - Add the Cloud Mastery event report <br> - Revise the high-availability architecture diagram: CloudFront, S3, ALB, ECS Fargate, ECR, NAT Gateway, S3 Gateway Endpoint, CloudWatch, CloudTrail, Supabase and external integrations                                                                                                                                                                 | 14/11/2026 | 14/11/2026      | NexusOMS workshop documents                                                 |
| 7   | - Practice the full CI/CD pipeline with GitHub Actions and AWS OIDC: create the IAM role and inline policy for GitHub Actions, configure repository variables/secrets, build and push the backend image to ECR, update the ECS service, deploy the frontend to S3 and invalidate CloudFront <br> - Fix errors with OIDC, Docker build, Secrets Manager and the ALB health check <br> - Adjust the workflow to publish only the backend container image <br> - Restructure the workshop content and images to the new flow | 15/11/2026 | 15/11/2026      | GitHub Actions <br> AWS Management Console <br> NexusOMS workshop documents |


### Week 9 Achievements:

* Prepared the backend container: fixed the Docker image with the Kerberos library for Npgsql and verified the container can connect to PostgreSQL/Supabase.

* Deployed NexusOMS to AWS:
  * Backend on Amazon ECS with AWS Fargate
  * Frontend on Amazon S3 and CloudFront
  * Fixed the backend HTTP 500 error, the frontend session timeout and the product selection by stock, and added the initial stock quantity when creating a product

* Built the AWS network and compute layer by hand: VPC, two public and two private subnets across two Availability Zones, Internet Gateway, two NAT Gateways, route tables, security groups, ALB, target group, ECS and Fargate. Solved the errors with subnets, IGW routes, security groups, Secrets Manager, the task role and health checks.

* Set up CI/CD from GitHub Actions to Amazon ECR and Amazon ECS/Fargate, and from GitHub Actions to S3 and CloudFront invalidation:
  * Used GitHub OIDC instead of long-lived AWS access keys, with an IAM role and inline policy for GitHub Actions
  * Configured repository variables and secrets
  * Fixed the OIDC, Docker build, Secrets Manager and ALB health check errors
  * Adjusted the workflow to publish only the backend container image

* Managed the cost of the experiments by cleaning up the AWS resources after failed deployments and after the demo.

* Completed the documentation:
  * Bilingual NexusOMS workshop with guide images, restructured to the new flow
  * Image file names normalized so they work on Vercel
  * High-availability architecture diagram with CloudFront, S3, ALB, ECS Fargate, ECR, NAT Gateway, S3 Gateway Endpoint, CloudWatch, CloudTrail, Supabase and external integrations
  * Cloud Mastery event report

* Result: NexusOMS now runs end to end on AWS with a two-Availability-Zone architecture and an automated deployment pipeline.