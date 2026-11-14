---
title: "Build and push the backend image"
date: 2026-11-13
weight: 2
chapter: false
---

Use one immutable tag per build. After pushing, copy the complete image URI including its tag for the task definition.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**ECR repository**

```text
$EcrRepositoryUri = '<AWS_ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/nexusoms-demo-api'
```

**Build and push the backend image - input 2**

```text
$ImageTag = 'v<YYYY-MM-DD>-01'
```

**PowerShell command**

```text
docker build -f .\backend\Dockerfile -t "nexusoms-demo-api:$ImageTag" .\backend
```

**PowerShell command**

```text
docker tag "nexusoms-demo-api:$ImageTag" "$EcrRepositoryUri`:$ImageTag"
```

**PowerShell command**

```text
docker push "$EcrRepositoryUri`:$ImageTag"
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Set up variable + Docker build BE

In AWS Console, complete **Set up variable + Docker build BE** as shown below.

**Values used in this step**

**PowerShell command**

```text
docker build -f .\backend\Dockerfile -t "nexusoms-demo-api:$ImageTag" .\backend
```

**PowerShell command**

```text
docker tag "nexusoms-demo-api:$ImageTag" "$EcrRepositoryUri`:$ImageTag"
```

**PowerShell command**

```text
docker push "$EcrRepositoryUri`:$ImageTag"
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/backend-image/1-set-up-variable-docker-build-be.png" title="Set up variable + Docker build BE" >}}

### Step 2 - Push Image to ECR Repository

In AWS Console, complete **Push Image to ECR Repository** as shown below.

**Values used in this step**

**ECR repository**

```text
$EcrRepositoryUri = '<AWS_ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/nexusoms-demo-api'
```

**Build and push the backend image - input 2**

```text
$ImageTag = 'v<YYYY-MM-DD>-01'
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/backend-image/2-push-image-to-ecr-repository.png" title="Push Image to ECR Repository" >}}
