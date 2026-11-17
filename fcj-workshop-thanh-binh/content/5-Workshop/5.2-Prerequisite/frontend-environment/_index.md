---
title: "Frontend environment variables"
date: 2026-11-15
weight: 3
chapter: false
---

Create `frontend/.env.local`.

![Create the frontend environment file](/images/5-Workshop/nexusoms/5.2-Prerequisite/frontend-environment/1-create-frontend-env-file.png)

```dotenv
VITE_DATA_MODE=supabase
VITE_API_BASE_URL=http://localhost:8080
VITE_SUPABASE_URL=https://<PROJECT_REF>.supabase.co
VITE_SUPABASE_ANON_KEY=<SUPABASE_PUBLISHABLE_OR_ANON_KEY>
```

![Configure frontend environment values](/images/5-Workshop/nexusoms/5.2-Prerequisite/frontend-environment/2-configure-frontend-env-values.png)
