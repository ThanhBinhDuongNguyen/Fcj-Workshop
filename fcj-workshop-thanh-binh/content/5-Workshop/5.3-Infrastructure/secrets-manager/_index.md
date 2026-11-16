---
title: "Secrets Manager"
date: 2026-11-13
weight: 9
chapter: false
---

Store all four values in one JSON secret. Do not create four separate secrets or expose real secret values in screenshots or Git.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**Secrets Manager - input 1**

```text
nexusoms/demo/application
```

**JSON content**

```text
{
```

**"postgresConnectionString"**

```text
"postgresConnectionString": "Host=<SESSION_POOLER_HOST>;Port=5432;Database=postgres;Username=<POOLER_USERNAME>;Password=<NEW_DATABASE_PASSWORD>;SSL Mode=Require;Trust Server Certificate=true",
```

**"supabaseUrl"**

```text
"supabaseUrl": "https://<PROJECT_REF>.supabase.co",
```

**"systemActorId"**

```text
"systemActorId": "<PROFILE_UUID>",
```

**"webhookSigningSecret"**

```text
"webhookSigningSecret": "<RANDOM_SECRET_AT_LEAST_32_CHARACTERS>"
```

**Secrets Manager - input 7**

```text
}
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open Secrets Manager service + Store a new secret

Complete **Open Secrets Manager service + Store a new secret**.

**Values used in this step**

**Secrets Manager - input 1**

```text
nexusoms/demo/application
```

**"postgresConnectionString"**

```text
"postgresConnectionString": "Host=<SESSION_POOLER_HOST>;Port=5432;Database=postgres;Username=<POOLER_USERNAME>;Password=<NEW_DATABASE_PASSWORD>;SSL Mode=Require;Trust Server Certificate=true",
```

**Secrets Manager - input 7**

```text
}
```

![Open Secrets Manager service + Store a new secret](/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/1-open-secrets-manager-service-store-a-new-secret.png)

### Step 2 - Choose secret type

Complete **Choose secret type**.

![Choose secret type](/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/2-choose-secret-type.png)

### Step 3 - Configure secret

Complete **Configure secret**.

**Values used in this step**

**JSON content**

```text
{
```

**"supabaseUrl"**

```text
"supabaseUrl": "https://<PROJECT_REF>.supabase.co",
```

**"systemActorId"**

```text
"systemActorId": "<PROFILE_UUID>",
```

**"webhookSigningSecret"**

```text
"webhookSigningSecret": "<RANDOM_SECRET_AT_LEAST_32_CHARACTERS>"
```

![Configure secret](/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/3-configure-secret.png)

### Step 4 - Copy secret ARN

Complete **Copy secret ARN**.

![Copy secret ARN](/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/4-copy-secret-arn.png)
