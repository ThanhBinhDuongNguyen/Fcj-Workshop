---
title: "Supabase project"
date: 2026-11-15
weight: 2
chapter: false
---

## 1. Create the project

Open Supabase Dashboard → **New project**.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/1-open-dashboard-and-create-project.png" title="Create a Supabase project" >}}

Select the organization, enter the project name and a strong database password, then select a region near Singapore. Save the database password.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/2-configure-database.png" title="Configure the database" >}}

## 2. Get connection values

Open **Project Settings → General** and copy the Project ID.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/3-copy-project-id.png" title="Copy the Project ID" >}}

Select **Connect**.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/4-open-connect.png" title="Open connection settings" >}}

Select **Session pooler** on port `5432`.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/5-select-session-pooler.png" title="Select Session pooler" >}}

Copy the host and username.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/6-copy-connection-values.png" title="Copy connection values" >}}

```text
Host=<SESSION_POOLER_HOST>;Port=5432;Database=postgres;Username=<POOLER_USERNAME>;Password=<DATABASE_PASSWORD>;SSL Mode=Require;Trust Server Certificate=true
```

Store the connection string securely.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/7-save-connection-string.png" title="Save the connection string" >}}

## 3. Run migrations

```powershell
npx supabase@latest login
```

Open the login URL displayed by Supabase CLI.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/8-connect-supabase-cli.png" title="Sign in to Supabase CLI" >}}

Copy the confirmation code into the terminal.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/9-copy-confirmation-code.png" title="Copy the confirmation code" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/10-cli-login-success.png" title="Supabase CLI login succeeded" >}}

Link the project and run migrations:

```powershell
npx supabase@latest link --project-ref "<PROJECT_REF>"
npx supabase@latest db push --remote
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/11-push-database-migrations.png" title="Push database migrations" >}}

Open **Table Editor** and verify that the tables exist.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/12-check-database-tables.png" title="Verify database tables" >}}

## 4. Create the demo account

Open **Authentication → Users → Add user**.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/13-open-authentication-and-add-user.png" title="Add a user" >}}

Create the demo email and password.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/14-create-demo-user.png" title="Create the demo user" >}}

Copy the user UUID, open `public.profiles`, and set the role:

```text
manager
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/15-update-user-role.png" title="Update the user role" >}}

Record these values:

```text
PROJECT_REF=<PROJECT_REF>
SUPABASE_URL=https://<PROJECT_REF>.supabase.co
SUPABASE_ANON_KEY=<SUPABASE_PUBLISHABLE_OR_ANON_KEY>
PROFILE_UUID=<PROFILE_UUID>
POSTGRES_CONNECTION_STRING=<POSTGRES_CONNECTION_STRING>
```
