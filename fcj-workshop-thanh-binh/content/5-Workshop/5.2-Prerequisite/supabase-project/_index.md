---
title: "Supabase project"
date: 2026-11-15
weight: 2
chapter: false
---

## 1. Create the project

Open Supabase Dashboard → **New project**.

![Create a Supabase project](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/1-open-dashboard-and-create-project.png)

Select the organization, enter the project name and a strong database password, then select a region near Singapore. Save the database password.

![Configure the database](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/2-configure-database.png)

## 2. Get connection values

Open **Project Settings → General** and copy the Project ID.

![Copy the Project ID](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/3-copy-project-id.png)

Select **Connect**.

![Open connection settings](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/4-open-connect.png)

Select **Session pooler** on port `5432`.

![Select Session pooler](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/5-select-session-pooler.png)

Copy the host and username.

![Copy connection values](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/6-copy-connection-values.png)

```text
Host=<SESSION_POOLER_HOST>;Port=5432;Database=postgres;Username=<POOLER_USERNAME>;Password=<DATABASE_PASSWORD>;SSL Mode=Require;Trust Server Certificate=true
```

Store the connection string securely.

![Save the connection string](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/7-save-connection-string.png)

## 3. Run migrations

```powershell
npx supabase@latest login
```

Open the login URL displayed by Supabase CLI.

![Sign in to Supabase CLI](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/8-connect-supabase-cli.png)

Copy the confirmation code into the terminal.

![Copy the confirmation code](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/9-copy-confirmation-code.png)

![Supabase CLI login succeeded](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/10-cli-login-success.png)

Link the project and run migrations:

```powershell
npx supabase@latest link --project-ref "<PROJECT_REF>"
npx supabase@latest db push --remote
```

![Push database migrations](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/11-push-database-migrations.png)

Open **Table Editor** and verify that the tables exist.

![Verify database tables](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/12-check-database-tables.png)

## 4. Create the demo account

Open **Authentication → Users → Add user**.

![Add a user](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/13-open-authentication-and-add-user.png)

Create the demo email and password.

![Create the demo user](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/14-create-demo-user.png)

Copy the user UUID, open `public.profiles`, and set the role:

```text
manager
```

![Update the user role](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/15-update-user-role.png)

Record these values:

```text
PROJECT_REF=<PROJECT_REF>
SUPABASE_URL=https://<PROJECT_REF>.supabase.co
SUPABASE_ANON_KEY=<SUPABASE_PUBLISHABLE_OR_ANON_KEY>
PROFILE_UUID=<PROFILE_UUID>
POSTGRES_CONNECTION_STRING=<POSTGRES_CONNECTION_STRING>
```
