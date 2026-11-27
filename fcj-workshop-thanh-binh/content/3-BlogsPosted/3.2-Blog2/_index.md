---
title: "Blog 2"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# ECS FARGATE FAILS TO START: DISTINGUISHING TASK ROLE AND EXECUTION ROLE

### Introduction

When the NexusOMS ASP.NET Core backend was deployed to ECS Fargate, the service existed but its tasks repeatedly stopped with ResourceInitializationError and AccessDenied for secretsmanager:GetSecretValue. This article uses that failure to distinguish the two IAM roles in a task definition.

### Key points

1. **Execution role:** ECS uses this role to pull images from ECR, send logs and retrieve secrets referenced by the task definition.
2. **Task role:** Application code uses this role when calling AWS APIs at runtime, such as sending and receiving SQS messages.
3. **Identify the required permission:** ECS retrieves the connection string from Secrets Manager before the container starts, so GetSecretValue belongs in the execution role. A customer managed KMS key also requires checking decrypt permissions.
4. **Read the error:** Identify the denied identity, action, resource and time of the call. Grant access to the correct secret instead of attaching AdministratorAccess.
5. **Verify the result:** Monitor the replacement task, Healthy targets and API responses. An HTTP 500 caused by an invalid connection string is investigated separately from IAM failures.

### Lessons learned

An Active service does not prove that the application works. Before adding a policy, identify who calls AWS, which action is called, which resource is accessed and when the call occurs. This helps correct the right role without granting excessive permissions.

### References

- [IAM for Amazon ECS on AWS Fargate - Wilklins Nyatteng](https://dev.to/aws-builders/iam-for-amazon-ecs-on-aws-fargate-2bk8)
- [Amazon ECS task execution IAM role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html)

### Published article and evidence

The Vietnamese article was approved and published in AWS Study Group.

[View the post on Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2347546992676926)

![Evidence of Blog 2 published in AWS Study Group](/images/3-Blog/blog2.png)
