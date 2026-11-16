---
title: "CloudFront distribution"
date: 2026-11-13
weight: 11
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

Complete **Open CloudFront dashboard + Create distribution**.

**Values used in this step**

**CloudFront distribution - input 1**

```text
nexusoms-demo-web
```

![Open CloudFront dashboard + Create distribution](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/1-open-cloudfront-dashboard-create-distribution.png)

### Step 2 - Configure distribution options

Complete **Configure distribution options**.

**Values used in this step**

**Default root object**

```text
Default root object: index.html
```

**Path pattern**

```text
Path pattern: /api/*
```

![Configure distribution options](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/2-configure-distribution-options.png)

### Step 3 - Choose S3 type + mapping to S3 FE

Complete **Choose S3 type + mapping to S3 FE**.


![Choose S3 type + mapping to S3 FE](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/3-choose-s3-type-mapping-to-s3-fe.png)

### Step 4 - Click next

Complete **Click next**.

![Click next](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/4-click-next.png)

### Step 5 - Choose Do not enable for demo

Complete **Choose Do not enable for demo**.

![Choose Do not enable for demo](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/5-choose-do-not-enable-for-demo.png)

### Step 6 - Click Create distribution

Complete **Click Create distribution**.

![Click Create distribution](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/6-click-create-distribution.png)

### Step 7 - Choose origins + Create origin

Complete **Choose origins + Create origin**.

![Choose origins + Create origin](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/7-choose-origins-create-origin.png)

### Step 8 - Configure setting for origin

Complete **Configure setting for origin**.

**Values used in this step**

**Origin name**

```text
nexusoms-demo-alb-origin
```

![Configure setting for origin](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/8-configure-setting-for-origin.png)

### Step 9 - Choose Behavior tab + Create behavior

Complete **Choose Behavior tab + Create behavior**.

![Choose Behavior tab + Create behavior](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/9-choose-behavior-tab-create-behavior.png)

### Step 10 - Configure behavior setting

Complete **Configure behavior setting**.

![Configure behavior setting](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/10-configure-behavior-setting.png)

### Step 11 - Choose origin request policy

Complete **Choose origin request policy**.

**Values used in this step**

**Cache policy**

```text
Cache policy: CachingDisabled
```

**Origin request policy**

```text
Origin request policy: AllViewerExceptHostHeader
```

![Choose origin request policy](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/11-choose-origin-request-policy.png)
