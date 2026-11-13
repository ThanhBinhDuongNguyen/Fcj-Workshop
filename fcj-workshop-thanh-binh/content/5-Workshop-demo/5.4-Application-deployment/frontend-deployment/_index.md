---
title: "Build and upload the frontend"
date: 2026-11-13
weight: 11
chapter: false
---

The production frontend must call the API through relative `/api` paths; do not build it with localhost or direct ALB URLs.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**Build and upload the frontend - input 1**

```text
VITE_API_BASE_URL=/api
```

**VITE_SUPABASE_URL=https**

```text
VITE_SUPABASE_URL=https://<PROJECT_REF>.supabase.co
```

**Supabase value**

```text
VITE_SUPABASE_ANON_KEY=<SUPABASE_PUBLISHABLE_KEY>
```

**PowerShell command**

```text
npm ci
```

**PowerShell command**

```text
npm run build
```

**PowerShell command**

```text
aws s3 sync .\dist "s3://nexusoms-demo-frontend-<AWS_ACCOUNT_ID>" --delete
```

**PowerShell command**

```text
aws cloudfront create-invalidation --distribution-id <DISTRIBUTION_ID> --paths "/*"
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Set up variable + run build project

In AWS Console, complete **Set up variable + run build project** as shown below.

**Values used in this step**

**Build and upload the frontend - input 1**

```text
VITE_API_BASE_URL=/api
```

**VITE_SUPABASE_URL=https**

```text
VITE_SUPABASE_URL=https://<PROJECT_REF>.supabase.co
```

**Supabase value**

```text
VITE_SUPABASE_ANON_KEY=<SUPABASE_PUBLISHABLE_KEY>
```

**PowerShell command**

```text
npm ci
```

**PowerShell command**

```text
npm run build
```

**PowerShell command**

```text
aws s3 sync .\dist "s3://nexusoms-demo-frontend-<AWS_ACCOUNT_ID>" --delete
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-deployment/1. Set up variable + run build project.png" title="Set up variable + run build project" >}}

### Step 2 - Push to S3

In AWS Console, complete **Push to S3** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-deployment/2. Push to S3.png" title="Push to S3" >}}

### Step 3 - Create invalidation CloudFront

In AWS Console, complete **Create invalidation CloudFront** as shown below.

**Values used in this step**

**PowerShell command**

```text
aws cloudfront create-invalidation --distribution-id <DISTRIBUTION_ID> --paths "/*"
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-deployment/3. Create invalidation CloudFront.png" title="Create invalidation CloudFront" >}}
