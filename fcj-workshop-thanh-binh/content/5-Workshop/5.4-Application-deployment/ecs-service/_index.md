---
title: "ECS Service"
date: 2026-11-13
weight: 8
chapter: false
---

Create an ECS Service that runs Fargate tasks in two private subnets and registers them with the ALB target group.

## Step 1: Create the service

Open **Amazon ECS → Clusters → nexusoms-demo-cluster → Services → Create**.

![Open the ECS Service creation page](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/1-choose-service-tab-create.png)

## Step 2: Configure the service

```text
Compute options: Launch type
Launch type: Fargate
Task definition family: nexusoms-demo-api
Service name: nexusoms-demo-api-service
Desired tasks: 1
```

Select the latest task definition revision. Increase **Desired tasks** to `2` when demonstrating two tasks.

With `Desired tasks = 1`, the task runs in only one Availability Zone at a time. Selecting two private subnets does not create a highly available two-AZ backend.

![Configure the ECS Service](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/2-configure-service-detail.png)

## Step 3: Configure networking

```text
VPC: nexusoms-demo-vpc
Subnets: private-a and private-b
Security Group: nexusoms-demo-task-sg
Auto-assign public IP: Disabled
```

![Configure service networking](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/3-configure-network-for-service.png)

## Step 4: Connect the Load Balancer

```text
Load balancer type: Application Load Balancer
Load balancer: nexusoms-demo-alb
Listener: HTTP:80
Target group: nexusoms-demo-api-tg
Container: api
Container port: 8080
```

![Select the Application Load Balancer](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/4-configure-load-balancer-for-service.png)

![Select the listener and target group](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/4-1-configure-load-balancer-for-service.png)

## Step 5: Finish

Choose **Create** and wait for the deployment to stabilize.

![Create the ECS Service](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/5-click-create.png)

Verify that:

- **Running tasks** equals **Desired tasks**.
- The target in `nexusoms-demo-api-tg` is **Healthy**.
- `/ecs/nexusoms-demo-api` contains a new CloudWatch log stream.
- The task has no public IP.

If the previous target becomes **Draining** during rollout, wait for the deployment to finish and check the new target.

| Change | Apply it by |
| --- | --- |
| Secret value | Force a new deployment |
| Environment, `ValueFrom`, or image | Register a new Task Definition revision and update the service |
| IAM policy | Verify the policy and start a new task |

The service is ready only when the deployment is **Completed** and the new target is **Healthy**. `Running` does not replace the health check.

## Step 6: Record the running task

```powershell
$TaskArn = aws ecs list-tasks --profile nexusoms-demo --region ap-southeast-1 --cluster nexusoms-demo-cluster --service-name nexusoms-demo-api-service --desired-status RUNNING --query "taskArns[0]" --output text
aws ecs describe-tasks --profile nexusoms-demo --region ap-southeast-1 --cluster nexusoms-demo-cluster --tasks $TaskArn --query "tasks[0].{TaskArn:taskArn,TaskDefinition:taskDefinitionArn,StartedAt:startedAt,LastStatus:lastStatus}" --output table
```

Open the task → **Logs** and record the latest log stream:

| Task ID | Task Definition revision | Started at | Log stream | Target |
| --- | --- | --- | --- | --- |
| `<TASK_ID>` | `<REVISION>` | `<STARTED_AT>` | `<LOG_STREAM>` | `Healthy` |
