---
title: "IAM roles for ECS"
date: 2026-11-13
weight: 10
chapter: false
---

The execution role lets ECS pull images, publish logs, and read the secret. Keep the task role minimal because the application does not call AWS APIs directly.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**ECS execution role**

```text
nexusoms-demo-ecs-execution-role
```

**Secret name**

```text
ReadNexusOmsApplicationSecret
```

**ECS task role**

```text
nexusoms-demo-ecs-task-role
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open Roles dashboard + Create role

In AWS Console, complete **Open Roles dashboard + Create role** as shown below.


{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/1. Open Roles dashboard + Create role.png" title="Open Roles dashboard + Create role" >}}

### Step 2 - Configure Select trusted entity

In AWS Console, complete **Configure Select trusted entity** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/2. Configure Select trusted entity.png" title="Configure Select trusted entity" >}}

### Step 3 - Configure permissions

In AWS Console, complete **Configure permissions** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/3. Configure permissions.png" title="Configure permissions" >}}

### Step 4 - Fill name role

In AWS Console, complete **Fill name role** as shown below.

**Values used in this step**

**ECS execution role**

```text
nexusoms-demo-ecs-execution-role
```

**Secret name**

```text
ReadNexusOmsApplicationSecret
```

**ECS task role**

```text
nexusoms-demo-ecs-task-role
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/4. Fill name role.png" title="Fill name role" >}}

### Step 5 - Find role + Open role

In AWS Console, complete **Find role + Open role** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/5. Find role + Open role.png" title="Find role + Open role" >}}

### Step 6 - Create inline policy

In AWS Console, complete **Create inline policy** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/6. Create inline policy.png" title="Create inline policy" >}}

### Step 7 - Paste the json to create

In AWS Console, complete **Paste the json to create** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/7. Paste the json to create.png" title="Paste the json to create" >}}
