---
title: "Frontend environment variables"
date: 2026-11-15
weight: 3
chapter: false
---

Create `frontend/.env.local`.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/frontend-environment/1-create-frontend-env-file.png" title="Create the frontend environment file" >}}

```dotenv
VITE_DATA_MODE=supabase
VITE_API_BASE_URL=http://localhost:8080
VITE_SUPABASE_URL=https://<PROJECT_REF>.supabase.co
VITE_SUPABASE_ANON_KEY=<SUPABASE_PUBLISHABLE_OR_ANON_KEY>
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/frontend-environment/2-configure-frontend-env-values.png" title="Configure frontend environment values" >}}

Do not commit `.env.local` to Git.
