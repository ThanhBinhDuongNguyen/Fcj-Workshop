---
title: "Blog 3"
date: 2026-11-27
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# DEPLOYING A .NET REST API ON ECS FARGATE: FROM DOCKER IMAGE TO RUNNING CONTAINER

For NexusOMS, the team packages the .NET REST API as a Docker image and deploys it on Amazon ECS Fargate. ECS orchestrates task lifecycles, while Fargate provides compute for the containers without requiring the team to manage EC2 servers.

### Deployment flow

1. GitHub Actions builds the Docker image from the backend source.
2. The pipeline tags the image by version and pushes it to Amazon ECR.
3. The pipeline registers a new Task Definition revision and updates the ECS Service.
4. Fargate starts the task and pulls the image from ECR.
5. The ALB checks task health before routing traffic to the new task.
6. CloudWatch Logs receives container logs for startup monitoring and troubleshooting.

### Key points

- **Task Definition:** specifies the image, CPU, memory, container port, environment variables, secrets, logging and IAM roles.
- **ECS Service:** maintains the desired task count. The two-AZ architecture requires two running tasks, one per AZ; the lab uses one task by default.
- **Networking:** the ALB uses two public subnets. Fargate runs in private subnets with `awsvpc`, a dedicated network interface per task and an `ip` target group.
- **IAM:** the execution role supports image pulls, secret retrieval and log delivery. The task role authorizes application calls to AWS APIs such as SQS.
- **Secrets Manager:** stores the connection string and webhook signing secret for ECS to inject at task startup.

### Lessons learned

A successful image push does not prove that the backend can serve requests. Verify the service's Task Definition revision, `RUNNING` tasks, `Healthy` targets and API responses. With the deployment circuit breaker and rollback enabled, ECS can return to the previous successful deployment if the new deployment fails.

### References

- [Task definitions for Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/fargate-tasks-services.html)
- [Amazon ECS overview](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)
- [Amazon ECS task definitions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html)
- [Amazon ECR overview](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)

The Vietnamese article was approved and published in AWS Study Group.

[View the post on Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2350934632338162)

![The .NET REST API on ECS Fargate article approved by AWS Study Group](/images/3-Blog/blog3.png)
