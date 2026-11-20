---
title: "Secrets Manager"
date: 2026-11-17
weight: 9
chapter: false
---

Store all four values in one JSON secret. Do not create four separate secrets.

## 1. Prepare the Values

| JSON key | Required value |
| --- | --- |
| `postgresConnectionString` | Npgsql Session pooler string with the database password |
| `supabaseUrl` | Project URL, not a database connection string |
| `systemActorId` | UUID of the active demo profile |
| `webhookSigningSecret` | Random string with at least 32 characters |

Get these values from Supabase:

- **Session pooler:** choose **Connect → Session pooler**, then copy the host, port, and username.
- **Database password:** use the Supabase database password.
- **Project URL:** open **Project Settings → API → Project URL**.
- **Profile UUID:** open **SQL Editor → New query** and run:

```sql
select id, email
from public.profiles
where lower(email) = lower('<LOGIN_EMAIL>')
limit 1;
```

Copy the UUID of the demo account. Do not generate a random UUID.

## 2. Create the Secret

Open **AWS Secrets Manager → Store a new secret**.

![Open Secrets Manager and create a secret](/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/1-open-secrets-manager-service-store-a-new-secret.png)

Select:

```text
Secret type: Other type of secret
```

Open the **Plaintext** tab. Replace every placeholder and paste the complete JSON block:

```json
{
  "postgresConnectionString": "Host=<SESSION_POOLER_HOST>;Port=5432;Database=postgres;Username=<POOLER_USERNAME>;Password=<DATABASE_PASSWORD>;SSL Mode=Require;Trust Server Certificate=true",
  "supabaseUrl": "https://<PROJECT_REF>.supabase.co",
  "systemActorId": "<PROFILE_UUID>",
  "webhookSigningSecret": "<RANDOM_SECRET_AT_LEAST_32_CHARACTERS>"
}
```
![Choose the secret type](/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/2-choose-secret-type.png)


Choose **Next** and enter:

```text
Secret name: nexusoms/demo/application
```
![Enter the JSON secret](/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/3-configure-secret.png)


Choose **Next → Next → Store**.

## 3. Copy the Secret ARN

Open the new secret and copy the complete **Secret ARN**.

![Copy the Secret ARN](/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/4-copy-secret-arn.png)

Save the ARN for the `nexusoms-demo-ecs-execution-role` policy in the next step.

## 4. Verify

Open **Secret value → Retrieve secret value** and confirm these four keys exist:

```text
postgresConnectionString
supabaseUrl
systemActorId
webhookSigningSecret
```

Validate the connection string format without printing the password:

```powershell
$AwsProfile = "nexusoms-demo"
$AwsRegion = "ap-southeast-1"
$SecretId = "nexusoms/demo/application"

$Secret = aws secretsmanager get-secret-value --profile $AwsProfile --region $AwsRegion --secret-id $SecretId --query SecretString --output text | ConvertFrom-Json
$Builder = [System.Data.Common.DbConnectionStringBuilder]::new()
$ParsedActorId = [guid]::Empty

try {
  $Builder.set_ConnectionString([string]$Secret.postgresConnectionString)
  "postgresConnectionString: valid"
} catch {
  "postgresConnectionString: invalid"
}

"supabaseUrl: $([uri]::IsWellFormedUriString([string]$Secret.supabaseUrl, [System.UriKind]::Absolute))"
"systemActorId: $([guid]::TryParse([string]$Secret.systemActorId, [ref]$ParsedActorId))"
"webhookSigningSecret: $(([string]$Secret.webhookSigningSecret).Length -ge 32)"
```

Continue only when all four checks are valid.

These checks validate the JSON structure, URL, UUID, and connection-string syntax only. Database connectivity is confirmed when a new ECS task starts, `/health` returns `200`, and the target group becomes `Healthy`.
