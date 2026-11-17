---
title: "Security groups"
date: 2026-11-13
weight: 7
chapter: false
---

The ALB accepts HTTP only from the CloudFront origin-facing prefix list; ECS accepts port 8080 only from the ALB security group.

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

### Step 1 - Open Security groups dashboard + Create security group


![Open Security groups dashboard + Create security group](/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/1-open-security-groups-dashboard-create-security-group.png)

### Step 2 - Full basic details of security group

**Values used in this step**

**CloudFront value**

```text
nexusoms-demo-alb-sg | TCP 80 | <CLOUDFRONT_ORIGIN_PREFIX_LIST_ID>
```

**Task definition value**

```text
nexusoms-demo-task-sg | TCP 8080 | nexusoms-demo-alb-sg
```

![Full basic details of security group](/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/2-full-basic-details-of-security-group.png)

### Step 3 - Configure the second security group

![Configure the second security group](/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/3-configure-the-second-security-group.png)
