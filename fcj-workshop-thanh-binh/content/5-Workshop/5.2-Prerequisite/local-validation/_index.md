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

## 6. Configure and run the backend locally

Open PowerShell in the repository root. Use the values saved in [Supabase project](../supabase-project/). The backend does not read `frontend/.env.local`.

To get `PROFILE_UUID`, open Supabase → **SQL Editor → New query** and run:

```sql
select id, email
from public.profiles
where lower(email) = lower('<LOGIN_EMAIL>')
  and is_active = true
limit 1;
```

Use the UUID of the demo account with the `manager` or `admin` role. Replace the placeholders and run the complete block:

```powershell
$env:ConnectionStrings__Postgres = 'Host=<SESSION_POOLER_HOST>;Port=5432;Database=postgres;Username=<POOLER_USERNAME>;Password=<DATABASE_PASSWORD>;SSL Mode=Require;Trust Server Certificate=true'
$env:Supabase__Url = "https://<PROJECT_REF>.supabase.co"
$env:Frontend__Origin = "http://localhost:5173"
$env:WebhookSimulator__SystemActorId = "<PROFILE_UUID>"
$env:WebhookSimulator__Secret = ([guid]::NewGuid().ToString("N") + [guid]::NewGuid().ToString("N"))
$env:WebhookQueue__Mode = "Direct"

dotnet run --project backend/src/NexusOMS.Api/NexusOMS.Api.csproj --launch-profile http --urls "http://localhost:5145"
```

Keep this window running. Set these variables again if you restart the backend in a new PowerShell window. `Direct` mode does not require SQS or AWS credentials.

## 7. Run the frontend and verify

Open a **second PowerShell window** in the repository root:

```powershell
Invoke-RestMethod -Method Get -Uri "http://localhost:5145/health"

Push-Location frontend
npm run dev -- --host localhost --port 5173 --strictPort
```

A `status: ok` response from `/health` only confirms that the backend responds.

Open `http://localhost:5173`, sign in with the demo account, and open the order list and order details. In **DevTools → Network**, verify that the `/api/v1/orders` request to `http://localhost:5145` returns `200` with JSON data. This verifies that the backend can read the database.

Open **Webhook Simulator → Order lifecycle**, select a channel connection, warehouse, and SKU mapping from the seed data, and send a create-order webhook. Wait for processing to finish, then open the new order.

Continue only after the build/test and local checks pass. Press `Ctrl+C` in each window to stop the backend and frontend.
