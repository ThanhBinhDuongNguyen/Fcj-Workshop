---
title: "ECS service"
date: 2026-11-13
weight: 7
chapter: false
---

Run one Fargate task across the private subnets, disable public IP assignment, and register the service with the ALB target group.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**ECS service name**

```text
nexusoms-demo-api-service
```

**Desired tasks**

```text
Desired tasks: 1
```

**Auto-assign public IP**

```text
Auto-assign public IP: Disabled
```

**Container**

```text
Container: api
```

**Container port**

```text
Container port: 8080
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Choose service tab + Create

In AWS Console, complete **Choose service tab + Create** as shown below.


{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/1-choose-service-tab-create.png" title="Choose service tab + Create" >}}

### Step 2 - Configure service detail

In AWS Console, complete **Configure service detail** as shown below.

**Values used in this step**

**ECS service name**

```text
nexusoms-demo-api-service
```

**Desired tasks**

```text
Desired tasks: 1
```

**Auto-assign public IP**

```text
Auto-assign public IP: Disabled
```

**Container**

```text
Container: api
```

**Container port**

```text
Container port: 8080
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/2-configure-service-detail.png" title="Configure service detail" >}}

### Step 3 - Configure Network for service

In AWS Console, complete **Configure Network for service** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/3-configure-network-for-service.png" title="Configure Network for service" >}}

### Step 4 - Configure Load balancer for service

In AWS Console, complete **Configure Load balancer for service** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/4-configure-load-balancer-for-service.png" title="Configure Load balancer for service" >}}

### Step 5 - Configure Load balancer for service

In AWS Console, complete **Configure Load balancer for service** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/4-1-configure-load-balancer-for-service.png" title="Configure Load balancer for service" >}}

### Step 6 - Click create

In AWS Console, complete **Click create** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/5-click-create.png" title="Click create" >}}
