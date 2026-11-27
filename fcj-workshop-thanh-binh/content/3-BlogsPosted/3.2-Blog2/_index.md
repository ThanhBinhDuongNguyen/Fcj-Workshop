---
title: "Blog 2"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---
# ECS FARGATE FAILS TO START: DISTINGUISHING TASK ROLE AND EXECUTION ROLE

When the NexusOMS ASP.NET Core backend was deployed to ECS Fargate, the service was created but its tasks repeatedly stopped with the following error:

> `ResourceInitializationError ... AccessDeniedException ... secretsmanager:GetSecretValue`

This failure clarified the responsibilities of the two IAM roles used by ECS.

### How the two roles differ

- The **Task Execution Role** is used by the ECS agent to pull an image from Amazon ECR, send logs and retrieve secrets referenced by the task definition.
- The **Task Role** is used by application code when it calls AWS APIs while running, such as receiving and processing messages from Amazon SQS.

In NexusOMS, the connection string is stored in AWS Secrets Manager and injected through the task definition. ECS must retrieve the secret before the container starts, so `secretsmanager:GetSecretValue` belongs in the execution role. Granting it only to the task role does not fix this initialization failure.

### Identifying the correct policy

For an IAM failure, the team checked four questions:

1. Which identity was denied?
2. Which action was denied?
3. Which resource was being accessed?
4. Did the call occur before container startup or while the application was running?

The error identified the execution role, the `GetSecretValue` action and the secret ARN. This allowed access to be limited to the correct secret instead of attaching `AdministratorAccess`. If the secret uses a customer managed KMS key, the role also requires the appropriate decrypt permission.

### Verification after the change

Fixing IAM only allows the task to pass the secret retrieval stage. The team also verified:

- The task reached `RUNNING`.
- The target group became `Healthy`.
- The endpoint responded.
- Database-backed features worked correctly.

After IAM was fixed, NexusOMS later returned HTTP 500 because the connection string format was invalid. This was an application configuration problem rather than a permissions issue. The main lesson is to identify **who is calling**, **which action is called**, **which resource is accessed** and **when the call occurs** before adding an IAM policy.

### References

- [IAM for Amazon ECS on AWS Fargate — Wilklins Nyatteng](https://dev.to/aws-builders/iam-for-amazon-ecs-on-aws-fargate-2bk8)
- [Amazon ECS task execution IAM role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html)

### Published article

[View the post on Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2347546992676926)


![The ECS Fargate article published in AWS Study Group](/images/3-Blog/blog2.png)
