---
title: "ECS task definition"
date: 2026-11-13
weight: 6
chapter: false
---

All four sensitive variables must use **ValueFrom** and reference their individual JSON keys in the same secret. Do not paste the entire JSON object as a value.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**ECS task definition - input 1**

```text
nexusoms-demo-api
```

**CPU**

```text
CPU: 0.5 vCPU
```

**Memory**

```text
Memory: 1 GB
```

**Container port**

```text
Container port: 8080
```

**ECS task definition - input 5**

```text
ASPNETCORE_ENVIRONMENT=Production
```

**Connection string**

```text
ConnectionStrings__Postgres <- postgresConnectionString
```

**Supabase value**

```text
Supabase__Url <- supabaseUrl
```

**System actor UUID**

```text
WebhookSimulator__SystemActorId <- systemActorId
```

**Webhook signing secret**

```text
WebhookSimulator__SigningSecret <- webhookSigningSecret
```

**ECS task definition - input 10**

```text
/ecs/nexusoms-demo-api
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open task definitions dashboard + Create new task definition

In AWS Console, complete **Open task definitions dashboard + Create new task definition** as shown below.


{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/1-open-task-definitions-dashboard-create-new-task-definition.png" title="Open task definitions dashboard + Create new task definition" >}}

### Step 2 - Configure task definition

In AWS Console, complete **Configure task definition** as shown below.

**Values used in this step**

**ECS task definition - input 1**

```text
nexusoms-demo-api
```

**CPU**

```text
CPU: 0.5 vCPU
```

**Memory**

```text
Memory: 1 GB
```

**ECS task definition - input 5**

```text
ASPNETCORE_ENVIRONMENT=Production
```

**Connection string**

```text
ConnectionStrings__Postgres <- postgresConnectionString
```

**Supabase value**

```text
Supabase__Url <- supabaseUrl
```

**System actor UUID**

```text
WebhookSimulator__SystemActorId <- systemActorId
```

**Webhook signing secret**

```text
WebhookSimulator__SigningSecret <- webhookSigningSecret
```

**ECS task definition - input 10**

```text
/ecs/nexusoms-demo-api
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/2-configure-task-definition.png" title="Configure task definition" >}}

### Step 3 - Configure OS, task roles

In AWS Console, complete **Configure OS, task roles** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/3-configure-os-task-roles.png" title="Configure OS, task roles" >}}

### Step 4 - Configure Container-1 detail

In AWS Console, complete **Configure Container-1 detail** as shown below.

**Values used in this step**

**Container port**

```text
Container port: 8080
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/4-configure-container-1-detail.png" title="Configure Container-1 detail" >}}

### Step 5 - Configure environment variables

In AWS Console, complete **Configure environment variables** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/5-configure-environment-variables.png" title="Configure environment variables" >}}
