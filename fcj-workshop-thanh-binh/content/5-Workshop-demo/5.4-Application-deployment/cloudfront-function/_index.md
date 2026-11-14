---
title: "CloudFront function for SPA routing"
date: 2026-11-13
weight: 10
chapter: false
---

The function rewrites extensionless frontend routes to `/index.html` while preserving `/api/*`, static assets, and query strings.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**CloudFront function for SPA routing - input 1**

```text
nexusoms-demo-spa-routing
```

**Runtime**

```text
Runtime: JavaScript 2.0
```

**Event**

```text
Event: Viewer request
```

**Behavior**

```text
Behavior: Default (*)
```

**Stage**

```text
Stage: LIVE
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open Functions dashboard + Create function

In AWS Console, complete **Open Functions dashboard + Create function** as shown below.


{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/1-open-functions-dashboard-create-function.png" title="Open Functions dashboard + Create function" >}}

### Step 2 - Configure function details

In AWS Console, complete **Configure function details** as shown below.

**Values used in this step**

**CloudFront function for SPA routing - input 1**

```text
nexusoms-demo-spa-routing
```

**Runtime**

```text
Runtime: JavaScript 2.0
```

**Event**

```text
Event: Viewer request
```

**Behavior**

```text
Behavior: Default (*)
```

**Stage**

```text
Stage: LIVE
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/2-configure-function-details.png" title="Configure function details" >}}

### Step 3 - Paste code build section

In AWS Console, complete **Paste code build section** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/3-paste-code-build-section.png" title="Paste code build section" >}}

### Step 4 - Publish function

In AWS Console, complete **Publish function** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/4-publish-function.png" title="Publish function" >}}

### Step 5 - Open distribution detail + choose behaviors + edit

In AWS Console, complete **Open distribution detail + choose behaviors + edit** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/5-open-distribution-detail-choose-behaviors-edit.png" title="Open distribution detail + choose behaviors + edit" >}}

### Step 6 - Configure Function associations

In AWS Console, complete **Configure Function associations** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/6-configure-function-associations.png" title="Configure Function associations" >}}

### Step 7 - Copy Distribution domain name

In AWS Console, complete **Copy Distribution domain name** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/7-copy-distribution-domain-name.png" title="Copy Distribution domain name" >}}
