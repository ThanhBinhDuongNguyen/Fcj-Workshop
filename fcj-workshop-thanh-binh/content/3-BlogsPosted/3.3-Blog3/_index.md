---
title: "Blog 3"
date: 2026-11-27
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# DEPLOYING A .NET REST API ON ECS FARGATE: FROM DOCKER IMAGE TO RUNNING CONTAINER

### Introduction

For NexusOMS, the team packages the .NET REST API as a Docker image and deploys it on Amazon ECS Fargate. The article follows the image build through to serving requests, with ECS orchestrating tasks and Fargate providing compute without requiring the team to manage EC2 servers.

### Key points

1. **Build and push the image:** GitHub Actions builds the backend, tags the image by version and pushes it to Amazon ECR.
2. **Task Definition:** The pipeline registers a new revision describing the image, CPU, memory, ports, environment variables, secrets, logging and IAM roles.
3. **ECS Service:** The service updates its revision and maintains the desired task count. The two-AZ architecture uses two tasks, one per AZ; the lab defaults to one task.
4. **Networking and health checks:** The ALB uses two public subnets. Fargate runs in private subnets with awsvpc, a network interface per task and an ip target group. The ALB checks health before routing traffic to a new task.
5. **Permissions and configuration:** The execution role supports image pulls, secret retrieval and logging; the task role grants SQS access to code. Secrets Manager supplies the connection string and webhook signing secret at startup.

### Lessons learned

Pushing an image does not prove the API works. Verify the Task Definition revision, RUNNING tasks, Healthy targets and API responses; use CloudWatch Logs to investigate failures. With the circuit breaker and rollback enabled, ECS can restore the previous successful deployment if the new deployment fails.

### References

- [Task definitions for Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/fargate-tasks-services.html)
- [Amazon ECS overview](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)
- [Amazon ECS task definitions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html)
- [Amazon ECR overview](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)

### Published article and evidence

The Vietnamese article was approved and published in AWS Study Group.

[View the post on Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2350934632338162)

![Evidence of Blog 3 published in AWS Study Group](/images/3-Blog/blog3.png)
