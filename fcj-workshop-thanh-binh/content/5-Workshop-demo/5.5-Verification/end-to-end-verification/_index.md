---
title: "End-to-end verification"
date: 2026-11-13
weight: 1
chapter: false
---

Verify the full path from CloudFront to S3, ALB, ECS, Supabase, and CloudWatch. Diagnose failures from status codes, target health, and logs instead of recreating resources.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**URL**

```text
https://<CLOUDFRONT_DOMAIN>/app
```

**URL**

```text
https://<CLOUDFRONT_DOMAIN>/api/health
```

**URL**

```text
https://<CLOUDFRONT_DOMAIN>/app?page=dashboard
```

**End-to-end verification - input 4**

```text
/ecs/nexusoms-demo-api
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open website

In AWS Console, complete **Open website** as shown below.


{{< figure src="/images/5-Workshop/nexusoms/5.5-Verification/end-to-end-verification/1. Open website.png" title="Open website" >}}

### Step 2 - Checking overall dashboard

In AWS Console, complete **Checking overall dashboard** as shown below.

**Values used in this step**

**URL**

```text
https://<CLOUDFRONT_DOMAIN>/app?page=dashboard
```

{{< figure src="/images/5-Workshop/nexusoms/5.5-Verification/end-to-end-verification/2. Checking overall dashboard.png" title="Checking overall dashboard" >}}

### Step 3 - Create product

In AWS Console, complete **Create product** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.5-Verification/end-to-end-verification/3. Create product.png" title="Create product" >}}

### Step 4 - Checking order detail

In AWS Console, complete **Checking order detail** as shown below.

**Values used in this step**

**URL**

```text
https://<CLOUDFRONT_DOMAIN>/app
```

**URL**

```text
https://<CLOUDFRONT_DOMAIN>/api/health
```

**End-to-end verification - input 4**

```text
/ecs/nexusoms-demo-api
```

{{< figure src="/images/5-Workshop/nexusoms/5.5-Verification/end-to-end-verification/4. Checking order detail.png" title="Checking order detail" >}}

### Step 5 - Measuring product cost

In AWS Console, complete **Measuring product cost** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.5-Verification/end-to-end-verification/4.1. Measuring product cost.png" title="Measuring product cost" >}}

### Step 6 - Using webhook to create order

In AWS Console, complete **Using webhook to create order** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.5-Verification/end-to-end-verification/5. Using webhook to create order.png" title="Using webhook to create order" >}}
