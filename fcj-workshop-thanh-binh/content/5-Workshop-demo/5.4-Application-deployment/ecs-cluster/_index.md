---
title: "ECS cluster"
date: 2026-11-13
weight: 3
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

In AWS Console, complete **Open Clusters dashboard + Create cluster** as shown below.


{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/1. Open Clusters dashboard + Create cluster.png" title="Open Clusters dashboard + Create cluster" >}}

### Step 2 - Configure cluster

In AWS Console, complete **Configure cluster** as shown below.

**Values used in this step**

**ECS cluster name**

```text
nexusoms-demo-cluster
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/2. Configure cluster.png" title="Configure cluster" >}}

### Step 3 - Choose update cluster

In AWS Console, complete **Choose update cluster** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/3. Choose update cluster.png" title="Choose update cluster" >}}

### Step 4 - Turn on Container Insights

In AWS Console, complete **Turn on Container Insights** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/4. Turn on Container Insights.png" title="Turn on Container Insights" >}}
