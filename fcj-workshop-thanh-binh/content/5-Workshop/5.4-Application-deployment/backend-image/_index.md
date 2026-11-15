---
title: "Build and push the backend image"
date: 2026-11-15
weight: 2
chapter: false
---

Open PowerShell in the repository root.

## 1. Log in to ECR and build the image

```powershell
$Region = "ap-southeast-1"
$AccountId = aws sts get-caller-identity `
  --profile nexusoms-demo `
  --query Account `
  --output text
$EcrRepositoryUri = "$AccountId.dkr.ecr.$Region.amazonaws.com/nexusoms-demo-api"
$Registry = $EcrRepositoryUri.Split('/')[0]
$ImageTag = "v1-initial"

aws ecr get-login-password `
  --profile nexusoms-demo `
  --region $Region |
  docker login --username AWS --password-stdin $Registry

docker build `
  --file backend/Dockerfile `
  --tag "nexusoms-demo-api:$ImageTag" `
  backend
```

The login result must be `Login Succeeded`, and the Docker build must complete.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/backend-image/1-bootstrap-login-and-build-backend.png" title="Log in to ECR and build the backend image" >}}

## 2. Tag and push the image

```powershell
docker tag `
  "nexusoms-demo-api:$ImageTag" `
  "$EcrRepositoryUri`:$ImageTag"

docker push "$EcrRepositoryUri`:$ImageTag"

$BackendImageUri = "$EcrRepositoryUri`:$ImageTag"
$BackendImageUri
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/backend-image/2-bootstrap-push-image-to-ecr.png" title="Push the backend image to ECR" >}}

Record `$BackendImageUri` for the task definition.
