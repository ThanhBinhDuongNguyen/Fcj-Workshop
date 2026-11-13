---
title: "Security groups"
date: 2026-11-13
weight: 7
chapter: false
---

The ALB accepts HTTP only from the CloudFront origin-facing prefix list; ECS accepts port 8080 only from the ALB security group.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**CloudFront value**

```text
nexusoms-demo-alb-sg | TCP 80 | <CLOUDFRONT_ORIGIN_PREFIX_LIST_ID>
```

**Task definition value**

```text
nexusoms-demo-task-sg | TCP 8080 | nexusoms-demo-alb-sg
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open Security groups dashboard + Create security group

In AWS Console, complete **Open Security groups dashboard + Create security group** as shown below.


{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/1. Open Security groups dashboard + Create security group.png" title="Open Security groups dashboard + Create security group" >}}

### Step 2 - Full basic details of security group

In AWS Console, complete **Full basic details of security group** as shown below.

**Values used in this step**

**CloudFront value**

```text
nexusoms-demo-alb-sg | TCP 80 | <CLOUDFRONT_ORIGIN_PREFIX_LIST_ID>
```

**Task definition value**

```text
nexusoms-demo-task-sg | TCP 8080 | nexusoms-demo-alb-sg
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/2. Full basic details of security group.png" title="Full basic details of security group" >}}

### Step 3 - Configure the second security group

In AWS Console, complete **Configure the second security group** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/3. Configure the second security group.png" title="Configure the second security group" >}}
