---
title: "Build and upload the frontend"
date: 2026-11-15
weight: 10
chapter: false
---

Open PowerShell in the repository root.

Keep the two CloudFront values separate:

| Value | Example | Used for |
| --- | --- | --- |
| Domain | `d....cloudfront.net` | `VITE_API_BASE_URL` |
| Distribution ID | `E...` | Invalidation command |

Verify that Git ignores the local production file:

```powershell
git check-ignore frontend/.env.production.local
```

## 1. Build the frontend

```powershell
$CloudFrontDomain = "<CLOUDFRONT_DOMAIN>"
$ProjectRef = "<PROJECT_REF>"
$SupabaseAnonKey = "<SUPABASE_PUBLISHABLE_OR_ANON_KEY>"

Push-Location frontend
$env:VITE_DATA_MODE = "supabase"
$env:VITE_API_BASE_URL = "https://$CloudFrontDomain"
$env:VITE_SUPABASE_URL = "https://$ProjectRef.supabase.co"
$env:VITE_SUPABASE_ANON_KEY = $SupabaseAnonKey

npm ci
npm run typecheck
npm run test
npm run build
```

Use only the Supabase publishable/anon key in a `VITE_` variable. Do not use a service-role or secret key.

![Configure variables and build the frontend](/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-deployment/1-set-up-variable-run-build-project.png)

## 2. Upload to S3

```powershell
$AccountId = aws sts get-caller-identity `
  --profile nexusoms-demo `
  --query Account `
  --output text
$BucketName = "nexusoms-demo-frontend-$AccountId"

aws s3 sync .\dist "s3://$BucketName" `
  --delete `
  --profile nexusoms-demo `
  --region ap-southeast-1
```

![Upload the frontend to S3](/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-deployment/2-push-to-s3.png)

## 3. Create a CloudFront invalidation

```powershell
$DistributionId = "<CLOUDFRONT_DISTRIBUTION_ID>"
$InvalidationId = aws cloudfront create-invalidation --distribution-id $DistributionId --paths "/*" --profile nexusoms-demo --query "Invalidation.Id" --output text
aws cloudfront wait invalidation-completed --distribution-id $DistributionId --id $InvalidationId --profile nexusoms-demo

Pop-Location
```

Continue only after the wait command completes successfully.

![Create a CloudFront invalidation](/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-deployment/3-create-invalidation-cloudfront.png)
