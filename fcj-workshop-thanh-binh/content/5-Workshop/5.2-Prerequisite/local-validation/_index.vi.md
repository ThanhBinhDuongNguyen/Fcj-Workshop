---
title: "Kiểm tra source local"
date: 2026-11-15
weight: 4
chapter: false
---

Mở PowerShell tại thư mục gốc của repository.

```powershell
dotnet restore backend/src/NexusOMS.Api/NexusOMS.Api.csproj
dotnet build backend/src/NexusOMS.Api/NexusOMS.Api.csproj -c Release
dotnet test backend/tests/NexusOMS.ApiTests/NexusOMS.ApiTests.csproj -c Release

Push-Location frontend
npm ci
npm run typecheck
npm run test
npm run build
Pop-Location

docker build --file backend/Dockerfile --tag nexusoms-api:local-check backend
```

![Kiểm tra source và Docker image](/images/5-Workshop/nexusoms/5.2-Prerequisite/local-validation/1-run-tests-and-docker-build.png)

Chỉ tiếp tục khi backend test, frontend test và Docker build đều thành công.

Khi chạy backend local, giữ chế độ mặc định:

```powershell
$env:WebhookQueue__Mode = "Direct"
dotnet run --project backend/src/NexusOMS.Api/NexusOMS.Api.csproj
```

Chế độ `Direct` không cần AWS SQS. Chỉ đặt `WebhookQueue__Mode=Sqs` trong ECS Task Definition sau khi hai queue đã được tạo.
