---
title: "Kiểm tra source local"
date: 2026-11-20
weight: 4
chapter: false
---

Hoàn thành checkpoint local trước khi tạo tài nguyên AWS có tính phí.

## 1. Kiểm tra công cụ

Mở PowerShell tại thư mục gốc của repository:

```powershell
git --version
gh --version
aws --version
dotnet --version
node --version
npm --version
docker --version
docker info
```

Mở Docker Desktop và chuyển sang Linux containers nếu `docker info` chưa kết nối được Docker Engine.

## 2. Kiểm tra quyền GitHub

Nếu thực hiện phần CI/CD, kiểm tra quyền trên repository fork:

```powershell
gh auth status
gh repo view <GITHUB_OWNER>/nexus-aws --json nameWithOwner,viewerPermission
```

`viewerPermission` phải là `ADMIN` để mở repository Settings, tạo Actions secrets/variables và cấu hình OIDC. Tài khoản cũng phải có quyền tạo branch và pull request.

## 3. Kiểm tra backend

```powershell
dotnet restore backend/src/NexusOMS.Api/NexusOMS.Api.csproj
dotnet build backend/src/NexusOMS.Api/NexusOMS.Api.csproj -c Release
dotnet test backend/tests/NexusOMS.ApiTests/NexusOMS.ApiTests.csproj -c Release
```

## 4. Kiểm tra frontend

```powershell
Push-Location frontend
npm ci
npm run typecheck
npm run test
npm run build
Pop-Location
```

Chờ `npm ci` kết thúc trước khi chạy lệnh tiếp theo.

## 5. Kiểm tra Docker image

```powershell
docker build --file backend/Dockerfile --tag nexusoms-api:local-check backend
```

![Kiểm tra source và Docker image](/images/5-Workshop/nexusoms/5.2-Prerequisite/local-validation/1-run-tests-and-docker-build.png)

## 6. Chạy backend local

```powershell
$env:WebhookQueue__Mode = "Direct"
dotnet run --project backend/src/NexusOMS.Api/NexusOMS.Api.csproj
```

Chế độ `Direct` không cần SQS. Chỉ tiếp tục khi backend build/test, frontend typecheck/test/build và Docker build đều thành công.
