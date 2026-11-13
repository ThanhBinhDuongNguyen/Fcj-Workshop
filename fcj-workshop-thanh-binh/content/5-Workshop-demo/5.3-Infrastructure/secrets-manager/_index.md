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

In AWS Console, complete **Open Secrets Manager service + Store a new secret** as shown below.

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

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/1. Open Secrets Manager service + Store a new secret.png" title="Open Secrets Manager service + Store a new secret" >}}

### Step 2 - Choose secret type

In AWS Console, complete **Choose secret type** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/2. Choose secret type.png" title="Choose secret type" >}}

### Step 3 - Configure secret

In AWS Console, complete **Configure secret** as shown below.

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

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/3. Configure secret.png" title="Configure secret" >}}

### Step 4 - Copy secret ARN

In AWS Console, complete **Copy secret ARN** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/4. Copy secret ARN.png" title="Copy secret ARN" >}}
