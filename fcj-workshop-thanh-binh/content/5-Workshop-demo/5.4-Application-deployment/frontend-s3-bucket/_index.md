---
title: "Frontend S3 bucket"
date: 2026-11-13
weight: 8
chapter: false
---

Keep the frontend bucket private. CloudFront accesses it through Origin Access Control; do not enable public static website hosting.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**Frontend resource name**

```text
nexusoms-demo-frontend-<AWS_ACCOUNT_ID>
```

**Block all public access**

```text
Block all public access: Enabled
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open S3 dashboard + Create bucket

In AWS Console, complete **Open S3 dashboard + Create bucket** as shown below.


{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-s3-bucket/1-open-s3-dashboard-create-bucket.png" title="Open S3 dashboard + Create bucket" >}}

### Step 2 - Configure bucket

In AWS Console, complete **Configure bucket** as shown below.

**Values used in this step**

**Frontend resource name**

```text
nexusoms-demo-frontend-<AWS_ACCOUNT_ID>
```

**Block all public access**

```text
Block all public access: Enabled
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-s3-bucket/2-configure-bucket.png" title="Configure bucket" >}}

### Step 3 - Click create bucket

In AWS Console, complete **Click create bucket** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-s3-bucket/3-click-create-bucket.png" title="Click create bucket" >}}
