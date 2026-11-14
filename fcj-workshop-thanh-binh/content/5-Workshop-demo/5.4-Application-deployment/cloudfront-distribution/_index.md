---
title: "CloudFront distribution"
date: 2026-11-13
weight: 9
chapter: false
---

Use S3 as the default origin and the ALB for `/api/*`. Forward Authorization/JWT while excluding the CloudFront Host header from ALB requests.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**CloudFront distribution - input 1**

```text
nexusoms-demo-web
```

**Default root object**

```text
Default root object: index.html
```

**Origin name**

```text
nexusoms-demo-alb-origin
```

**Path pattern**

```text
Path pattern: /api/*
```

**Cache policy**

```text
Cache policy: CachingDisabled
```

**Origin request policy**

```text
Origin request policy: AllViewerExceptHostHeader
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open CloudFront dashboard + Create distribution

In AWS Console, complete **Open CloudFront dashboard + Create distribution** as shown below.

**Values used in this step**

**CloudFront distribution - input 1**

```text
nexusoms-demo-web
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/1-open-cloudfront-dashboard-create-distribution.png" title="Open CloudFront dashboard + Create distribution" >}}

### Step 2 - Configure distribution options

In AWS Console, complete **Configure distribution options** as shown below.

**Values used in this step**

**Default root object**

```text
Default root object: index.html
```

**Path pattern**

```text
Path pattern: /api/*
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/2-configure-distribution-options.png" title="Configure distribution options" >}}

### Step 3 - Choose S3 type + mapping to S3 FE

In AWS Console, complete **Choose S3 type + mapping to S3 FE** as shown below.


{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/3-choose-s3-type-mapping-to-s3-fe.png" title="Choose S3 type + mapping to S3 FE" >}}

### Step 4 - Click next

In AWS Console, complete **Click next** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/4-click-next.png" title="Click next" >}}

### Step 5 - Choose Do not enable for demo

In AWS Console, complete **Choose Do not enable for demo** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/5-choose-do-not-enable-for-demo.png" title="Choose Do not enable for demo" >}}

### Step 6 - Click Create distribution

In AWS Console, complete **Click Create distribution** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/6-click-create-distribution.png" title="Click Create distribution" >}}

### Step 7 - Choose origins + Create origin

In AWS Console, complete **Choose origins + Create origin** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/7-choose-origins-create-origin.png" title="Choose origins + Create origin" >}}

### Step 8 - Configure setting for origin

In AWS Console, complete **Configure setting for origin** as shown below.

**Values used in this step**

**Origin name**

```text
nexusoms-demo-alb-origin
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/8-configure-setting-for-origin.png" title="Configure setting for origin" >}}

### Step 9 - Choose Behavior tab + Create behavior

In AWS Console, complete **Choose Behavior tab + Create behavior** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/9-choose-behavior-tab-create-behavior.png" title="Choose Behavior tab + Create behavior" >}}

### Step 10 - Configure behavior setting

In AWS Console, complete **Configure behavior setting** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/10-configure-behavior-setting.png" title="Configure behavior setting" >}}

### Step 11 - Choose origin request policy

In AWS Console, complete **Choose origin request policy** as shown below.

**Values used in this step**

**Cache policy**

```text
Cache policy: CachingDisabled
```

**Origin request policy**

```text
Origin request policy: AllViewerExceptHostHeader
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/11-choose-origin-request-policy.png" title="Choose origin request policy" >}}
