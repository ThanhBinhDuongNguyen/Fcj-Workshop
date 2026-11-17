---
title: "IAM roles for ECS"
date: 2026-11-13
weight: 11
chapter: false
---

The execution role lets ECS pull images, publish logs, and read the secret. Create the task role without a policy here; SQS access is added after both queues exist.

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

Do not attach `AdministratorAccess`. The next page adds a queue-scoped inline policy to this task role. The running .NET application uses the task role, while ECS uses the execution role during task startup.

## AWS Console walkthrough

### Step 1 - Open Roles dashboard + Create role


![Open Roles dashboard + Create role](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/1-open-roles-dashboard-create-role.png)

### Step 2 - Configure Select trusted entity

![Configure Select trusted entity](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/2-configure-select-trusted-entity.png)

### Step 3 - Configure permissions

![Configure permissions](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/3-configure-permissions.png)

### Step 4 - Fill name role

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

![Fill name role](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/4-fill-name-role.png)

### Step 5 - Find role + Open role

![Find role + Open role](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/5-find-role-open-role.png)

### Step 6 - Create inline policy

![Create inline policy](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/6-create-inline-policy.png)

### Step 7 - Paste the json to create

![Paste the json to create](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/7-paste-the-json-to-create.png)
