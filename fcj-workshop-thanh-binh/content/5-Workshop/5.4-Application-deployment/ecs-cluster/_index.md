---
title: "ECS cluster"
date: 2026-11-13
weight: 6
chapter: false
---

Create an ECS cluster for AWS Fargate. Container Insights is optional and can add observability cost.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**ECS cluster name**

```text
nexusoms-demo-cluster
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open Clusters dashboard + Create cluster

Complete **Open Clusters dashboard + Create cluster**.


![Open Clusters dashboard + Create cluster](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/1-open-clusters-dashboard-create-cluster.png)

### Step 2 - Configure cluster

Complete **Configure cluster**.

**Values used in this step**

**ECS cluster name**

```text
nexusoms-demo-cluster
```

![Configure cluster](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/2-configure-cluster.png)

### Step 3 - Choose update cluster

Complete **Choose update cluster**.

![Choose update cluster](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/3-choose-update-cluster.png)

### Step 4 - Turn on Container Insights

Complete **Turn on Container Insights**.

![Turn on Container Insights](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/4-turn-on-container-insights.png)
