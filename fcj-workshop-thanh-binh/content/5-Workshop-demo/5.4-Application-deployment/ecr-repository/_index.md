---
title: "ECR repository"
date: 2026-11-13
weight: 1
chapter: false
---

Create a private repository, enable immutable tags and scan on push, and copy the URI from the console instead of typing it manually.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**ECR repository - input 1**

```text
nexusoms-demo-api
```

**Private subnet value**

```text
Private
```

**ECR repository - input 3**

```text
Immutable
```

**Scan on push**

```text
Scan on push: Enabled
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open ECR dashboard and create repository

In AWS Console, complete **Open ECR dashboard and create repository** as shown below.

**Values used in this step**

**ECR repository - input 1**

```text
nexusoms-demo-api
```

**ECR repository - input 3**

```text
Immutable
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/1. Open ECR dashboard and create repository.png" title="Open ECR dashboard and create repository" >}}

### Step 2 - Configure private repository

In AWS Console, complete **Configure private repository** as shown below.

**Values used in this step**

**Private subnet value**

```text
Private
```

**Scan on push**

```text
Scan on push: Enabled
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/2. Configure private repository.png" title="Configure private repository" >}}

### Step 3 - Confirm encryption and create repository

In AWS Console, complete **Confirm encryption and create repository** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/3. Confirm encryption and create repository.png" title="Confirm encryption and create repository" >}}

### Step 4 - Copy ECR repository URI

In AWS Console, complete **Copy ECR repository URI** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/4. Copy ECR repository URI.png" title="Copy ECR repository URI" >}}
