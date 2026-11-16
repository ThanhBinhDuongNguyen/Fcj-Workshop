---
title: "Local source validation"
date: 2026-11-15
weight: 4
chapter: false
---

Run these commands from the repository root.

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

![Validate the source and Docker image](/images/5-Workshop/nexusoms/5.2-Prerequisite/local-validation/1-run-tests-and-docker-build.png)

Keep the default direct mode when running the backend locally:

```powershell
$env:WebhookQueue__Mode = "Direct"
dotnet run --project backend/src/NexusOMS.Api/NexusOMS.Api.csproj
```

`Direct` mode does not require AWS SQS. Set `WebhookQueue__Mode=Sqs` only in the ECS task definition after both queues exist.
