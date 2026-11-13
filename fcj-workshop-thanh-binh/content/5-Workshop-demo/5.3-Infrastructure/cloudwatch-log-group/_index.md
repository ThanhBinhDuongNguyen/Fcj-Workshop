---
title: "CloudWatch log group"
date: 2026-11-13
weight: 11
chapter: false
---

In the current console, open **CloudWatch → Logs Management → Create log group**. The task definition writes application logs to this group.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**CloudWatch log group - input 1**

```text
/ecs/nexusoms-demo-api
```

**CloudWatch log group - input 2**

```text
14 days
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open Log Management dashboard + Create log group

In AWS Console, complete **Open Log Management dashboard + Create log group** as shown below.

**Values used in this step**

**CloudWatch log group - input 1**

```text
/ecs/nexusoms-demo-api
```

**CloudWatch log group - input 2**

```text
14 days
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudwatch-log-group/1. Open Log Management dashboard + Create log group.png" title="Open Log Management dashboard + Create log group" >}}

### Step 2 - Fill name + retention setting

In AWS Console, complete **Fill name + retention setting** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudwatch-log-group/2. Fill name + retention setting.png" title="Fill name + retention setting" >}}
