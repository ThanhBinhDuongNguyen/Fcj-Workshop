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

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/local-validation/1-run-tests-and-docker-build.png" title="Kiểm tra source và Docker image" >}}

Chỉ tiếp tục khi backend test, frontend test và Docker build đều thành công.
