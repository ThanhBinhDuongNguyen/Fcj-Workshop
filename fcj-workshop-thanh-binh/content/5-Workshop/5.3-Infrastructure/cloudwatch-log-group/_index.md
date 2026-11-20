---
title: "CloudWatch Log Group"
date: 2026-11-20
weight: 10
chapter: false
---

Create the log group before registering the ECS Task Definition.

## Step 1: Create the log group

Open **CloudWatch → Logs → Log groups → Create log group**.

```text
Log group name: /ecs/nexusoms-demo-api
```

![Open the log group creation page](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudwatch-log-group/1-open-log-management-dashboard-create-log-group.png)

## Step 2: Set retention

Open the new log group and set:

```text
Retention setting: 14 days
```

![Set log retention](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudwatch-log-group/2-fill-name-retention-setting.png)

Verify that `/ecs/nexusoms-demo-api` exists before starting an ECS task.
