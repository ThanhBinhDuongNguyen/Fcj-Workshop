---
title: "Build và upload frontend"
date: 2026-11-15
weight: 10
chapter: false
---

Mở PowerShell tại thư mục gốc của repository.

## 1. Build frontend

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

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-deployment/1-set-up-variable-run-build-project.png" title="Cấu hình biến và build frontend" >}}

## 2. Upload lên S3

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

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-deployment/2-push-to-s3.png" title="Upload frontend lên S3" >}}

## 3. Tạo CloudFront invalidation

```powershell
$DistributionId = "<CLOUDFRONT_DISTRIBUTION_ID>"

aws cloudfront create-invalidation `
  --distribution-id $DistributionId `
  --paths "/*" `
  --profile nexusoms-demo

Pop-Location
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-deployment/3-create-invalidation-cloudfront.png" title="Tạo CloudFront invalidation" >}}
