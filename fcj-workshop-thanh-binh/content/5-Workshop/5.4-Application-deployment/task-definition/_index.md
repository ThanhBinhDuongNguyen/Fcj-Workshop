---
title: "ECS Task Definition"
date: 2026-11-13
weight: 7
chapter: false
---

Create the API task definition, read secrets from Secrets Manager, and send logs to CloudWatch.

## Step 1: Create the CloudWatch log group

Run in PowerShell:

```powershell
$AwsProfile = "nexusoms-demo"
$AwsRegion = "ap-southeast-1"
$LogGroupName = "/ecs/nexusoms-demo-api"

aws logs create-log-group `
  --profile $AwsProfile `
  --region $AwsRegion `
  --log-group-name $LogGroupName

aws logs put-retention-policy `
  --profile $AwsProfile `
  --region $AwsRegion `
  --log-group-name $LogGroupName `
  --retention-in-days 7
```

Continue if the log group already exists.

## Step 2: Open the task definition creation page

Open **Amazon ECS → Task definitions → Create new task definition**.

![Open the task definition creation page](/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/1-open-task-definitions-dashboard-create-new-task-definition.png)

## Step 3: Configure the task definition

```text
Task definition family: nexusoms-demo-api
Launch type: AWS Fargate
Operating system: Linux
CPU architecture: X86_64
CPU: 0.5 vCPU
Memory: 1 GB
Task role: nexusoms-demo-ecs-task-role
Task execution role: nexusoms-demo-ecs-execution-role
```

![Configure the task definition](/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/2-configure-task-definition.png)

![Select the operating system and IAM roles](/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/3-configure-os-task-roles.png)

## Step 4: Configure the container

Enter the complete ECR image URI, including its tag.

```text
Container name: api
Container port: 8080
Protocol: TCP
```

![Configure the container](/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/4-configure-container-1-detail.png)

## Step 5: Add environment variables

Add these variables with **Value type = Value**:

```text
ASPNETCORE_ENVIRONMENT=Production
ASPNETCORE_HTTP_PORTS=8080
AWS__Region=ap-southeast-1
WebhookQueue__Mode=Sqs
WebhookQueue__QueueUrl=https://sqs.ap-southeast-1.amazonaws.com/<ACCOUNT_ID>/nexusoms-demo-webhook-queue.fifo
WebhookQueue__DeadLetterQueueUrl=https://sqs.ap-southeast-1.amazonaws.com/<ACCOUNT_ID>/nexusoms-demo-webhook-dlq.fifo
WebhookQueue__WaitTimeSeconds=20
```

The two queue values must be **Queue URLs**, not ARNs.

Add four entries with **Value type = ValueFrom** and select the matching JSON key from `nexusoms/demo/application`:

| Task definition key | Secret JSON key |
| --- | --- |
| `ConnectionStrings__Postgres` | `postgresConnectionString` |
| `Supabase__Url` | `supabaseUrl` |
| `WebhookSimulator__Secret` | `webhookSigningSecret` |
| `WebhookSimulator__SystemActorId` | `systemActorId` |

![Configure environment variables](/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/5-configure-environment-variables.png)

## Step 6: Configure logging

Enable **Use log collection** and enter:

```text
Destination: Amazon CloudWatch
awslogs-group: /ecs/nexusoms-demo-api
awslogs-region: ap-southeast-1
awslogs-stream-prefix: ecs
awslogs-create-group: false
```

Choose **Create** and record the new revision, for example `nexusoms-demo-api:1`.
