---
title: "Validate the source locally"
date: 2026-11-20
weight: 4
chapter: false
---

Complete the local checkpoint before creating billable AWS resources.

## 1. Check the tools

Open PowerShell in the repository root:

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

Start Docker Desktop and use Linux containers if `docker info` cannot connect to the Docker Engine.

## 2. Verify GitHub permissions

For CI/CD, verify permissions on your fork:

```powershell
gh auth status
gh repo view <GITHUB_OWNER>/nexus-aws --json nameWithOwner,viewerPermission
```

`viewerPermission` must be `ADMIN` so you can open repository Settings, create Actions secrets and variables, and configure OIDC. The account must also be able to create branches and pull requests.

## 3. Validate the backend

```powershell
dotnet restore backend/src/NexusOMS.Api/NexusOMS.Api.csproj
dotnet build backend/src/NexusOMS.Api/NexusOMS.Api.csproj -c Release
dotnet test backend/tests/NexusOMS.ApiTests/NexusOMS.ApiTests.csproj -c Release
```

## 4. Validate the frontend

```powershell
Push-Location frontend
npm ci
npm run typecheck
npm run test
npm run build
Pop-Location
```

Wait for `npm ci` to finish before running the next command.

## 5. Validate the Docker image

```powershell
docker build --file backend/Dockerfile --tag nexusoms-api:local-check backend
```

![Validate the source and Docker image](/images/5-Workshop/nexusoms/5.2-Prerequisite/local-validation/1-run-tests-and-docker-build.png)

## 6. Run the backend locally

```powershell
$env:WebhookQueue__Mode = "Direct"
dotnet run --project backend/src/NexusOMS.Api/NexusOMS.Api.csproj
```

`Direct` mode does not require SQS. Continue only after the backend, frontend, and Docker checks pass.
