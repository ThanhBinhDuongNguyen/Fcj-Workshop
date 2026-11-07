---
title: "Week 7 Worklog"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Week 7 Objectives:
 
* Consolidate the NexusOMS plan and clearly separate the current MVP state from the target backend architecture.
* Finalize the canonical database schema and the backend project structure.
* Build the complete React frontend MVP and a Supabase database prototype.
* Prepare the backend completion guide (sprints, tests, definition of done) for the .NET implementation next week.
### Tasks to be carried out this week:
 
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Start Date | Completion Date | Reference Material                                                             |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------ |
| 1   | - Rewrite the unified plan (v4.0): <br>&emsp; + Separate the current MVP state (frontend, mock engine, Supabase migrations) from the target backend architecture (.NET modular monolith, Docker/AWS) <br>&emsp; + Freeze the MVP scope: 14 runtime screens grouped into 12 business areas <br>&emsp; + Define 4 MVP roles (Owner, Operations, Finance, Viewer) and role-based data redaction <br>&emsp; + Define 5 delivery phases and the target end-to-end demo flow                                                                                                                                            | 26/10/2026 | 26/10/2026      | NexusOMS unified plan (v4.0) <br> NexusOMS unified plan (v3.0, for comparison) |
| 2   | - Write the backend completion guide: <br>&emsp; + Module boundaries: Identity and Tenancy, Inventory and Channels, Ordering and Fulfillment, Finance and Reporting, Integration and Async <br>&emsp; + API and transaction rules: server-side tenant context, membership and role checks, idempotency keys, ledger-only stock changes, webhook flow (verify HMAC → persist → enqueue → worker → retry/DLQ) <br>&emsp; + Minimal inventory contract: resolve SKU, reserve, commit outbound, release, return inbound <br>&emsp; + 6 sprints (S1–S6) with acceptance criteria, test strategy and definition of done | 27/10/2026 | 27/10/2026      | NexusOMS backend completion guide <br> NexusOMS unified plan (v4.0)            |
| 3   | - Design the database and project structure: <br>&emsp; + Rewrite `Database.dbml` as the canonical schema aligned with Supabase migrations (PostgreSQL 16, public schema, every business table tenant-scoped) <br>&emsp; + 24 tables and 6 enums covering identity and tenancy, catalog and inventory ledger, channels, orders, returns, finance journal, webhook delivery logs and audit logs <br>&emsp; + Modular Monolith solution layout: `NexusOMS.Api`, `NexusOMS.Contracts`, `Modules/InventoryAndChannels`, `Modules/OrderingAndFinance`                                                                  | 28/10/2026 | 28/10/2026      | `Database.dbml` <br> Project structure document <br> Supabase migrations       |
| 4   | - Build the frontend MVP (part 1): <br>&emsp; + React 18 + TypeScript + Vite application, running locally on port 5174 <br>&emsp; + Role-based navigation and data redaction (Owner, Operations, Finance, Viewer) <br>&emsp; + Mock engine as the default data source <br>&emsp; + Data adapter layer to switch services from mock to Supabase/API                                                                                                                                                                                                                                                                | 29/10/2026 | 29/10/2026      | NexusOMS frontend repository <br> NexusOMS unified plan (v4.0)                 |
| 5   | - Build the frontend MVP (part 2): <br>&emsp; + 14 runtime screens in 12 business areas: dashboard, inventory, orders, fulfillment, returns, customers, order economics, SKU economics, channel economics, cash and working capital, pricing and channel connection <br>&emsp; + Channel connection area with 3 screens: connection, SKU mapping and Flash Sale simulator <br>&emsp; + Run the business flows end to end through the mock engine                                                                                                                                                                  | 30/10/2026 | 30/10/2026      | NexusOMS frontend repository <br> NexusOMS unified plan (v4.0)                 |
| 6   | - Set up Supabase as the prototype data layer: <br>&emsp; + PostgreSQL on Supabase following the migrations: multi-tenant schema with RLS/RPC <br>&emsp; + Keep the mock engine as a fallback when no infrastructure is available <br> - Week wrap-up and preparation for the .NET backend                                                                                                                                                                                                                                                                                                                        | 31/10/2026 | 31/10/2026      | Supabase project and migrations <br> `Database.dbml`                           |
 
 
### Week 7 Achievements:
 
* Produced a single, honest NexusOMS plan (v4.0) that states what already exists (React frontend with a mock engine, Supabase migrations) and what is still target architecture (.NET backend, Docker, AWS), so the report does not overstate the current state.
* Defined the MVP scope and access model:
  * 14 runtime screens grouped into 12 business areas
  * 4 roles with role-specific data redaction (for example, Operations cannot see P&L or cash flow)
  * 5 delivery phases: local MVP → data foundation → .NET API → async and storage → AWS deployment
* Wrote the backend completion guide:
  * Clear module boundaries and ownership
  * Server-side rules for tenant context, authorization, idempotency and ledger-only stock changes
  * A secure webhook flow: verify HMAC → persist delivery → enqueue → worker → retry/DLQ
  * 6 sprints with measurable acceptance criteria (for example, a concurrent order test must never produce negative stock, and replaying a webhook must not create side effects twice)
  * Test strategy covering unit, integration, API, E2E and load/concurrency tests
* Replaced the earlier 14-table, 3-schema design with a canonical multi-tenant schema of 24 tables and 6 enums, aligned with the Supabase migrations so the .NET backend inherits one schema instead of two incompatible ones.
* Finalized the Modular Monolith project structure for the backend (`NexusOMS.Api`, `NexusOMS.Contracts` and the module folders).
* Completed the frontend MVP (React 18, TypeScript, Vite) with role-based navigation, a mock engine for local demos and an adapter layer that allows moving services to Supabase/API one by one without rewriting the UI.
* Set up the Supabase prototype for the data layer, ready for the data foundation phase.
* Next week: implement the .NET backend following the completion guide, starting with S1 (schema and auth, API skeleton).