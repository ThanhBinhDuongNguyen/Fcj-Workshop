---
title: "Target Group"
date: 2026-11-20
weight: 3
chapter: false
---

Create an IP target group for ECS Fargate.

## Step 1: Create the target group

Open **EC2 → Target Groups → Create target group**.

![Open the target group creation page](/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/1-open-target-groups-dashboard-create-target-group.png)

## Step 2: Configure the target group

```text
Target type: IP addresses
Target group name: nexusoms-demo-api-tg
Protocol: HTTP
Port: 8080
IP address type: IPv4
VPC: nexusoms-demo-vpc
```

![Configure the target group](/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/2-configure-target-group.png)

## Step 3: Configure the health check

```text
Health check protocol: HTTP
Health check path: /health
Success codes: 200
```

![Configure the health check](/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/3-choose-vpc-set-up-health-checks.png)

## Step 4: Finish

Choose **Next**. Do not register IP addresses manually. Choose **Create target group**.

![Create the target group](/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/4-complete-create-target-group.png)

Verify that the target group uses `IP addresses`, port `8080`, and `/health` returns `200` after the ECS Service starts.
