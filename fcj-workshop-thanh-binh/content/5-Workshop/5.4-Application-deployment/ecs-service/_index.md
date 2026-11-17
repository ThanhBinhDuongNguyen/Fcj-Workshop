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
