---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:
 
* Simplify the NexusOMS scope to what the MVP needs and move from a multi-tenant model to a single-organization model.
* Build and test the .NET backend on Supabase (Auth + PostgreSQL) for the core product, warehouse, inventory and order flows.
* Connect the frontend to the real API and prepare an end-to-end demo (webhook simulator, flash sale, profit reports).
* Review the target AWS architecture and update the proposal documents.
### Tasks to be carried out this week:
 
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                    | Start Date | Completion Date | Reference Material                                                    |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | --------------------------------------------------------------------- |
| 1   | - Review the previous project and cut the features and pages that do not serve the MVP <br> - Decide to keep Supabase and switch from multi-tenant to a single organization                                                                                                                                                                                                                             | 01/11/2026 | 01/11/2026      | NexusOMS repository                                                   |
| 2   | - Complete the single-organization backend foundation <br>&emsp; + Build Brand CRUD <br>&emsp; + Set up the .NET backend structure, Docker and basic CI/CD                                                                                                                                                                                                                                              | 02/11/2026 | 02/11/2026      | NexusOMS repository                                                   |
| 3   | - Set up a new Supabase project: create the baseline migration, configure `.env.local`, link the Supabase project and check Git ignore rules for secrets <br> - Build the Product, Warehouse and Inventory (bins/channels) APIs <br> - Create the Postman testing flow                                                                                                                                  | 03/11/2026 | 03/11/2026      | NexusOMS repository <br> Supabase project and migrations <br> Postman |
| 4   | - Complete the order–inventory flow: create order, reserve stock, export stock, cancel order and deliver <br> - Add transactions, idempotency, migrations and tests                                                                                                                                                                                                                                     | 04/11/2026 | 04/11/2026      | NexusOMS repository <br> Postman                                      |
| 5   | - Connect the frontend to the real API <br>&emsp; + Complete customers, channel connection, P&L CM1–CM3 and the dashboard <br>&emsp; + Fix token/auth errors <br> - Start the webhook simulator and clean up legacy mock code                                                                                                                                                                           | 05/11/2026 | 05/11/2026      | NexusOMS repository                                                   |
| 6   | - Complete the demo experience: staged webhooks, inventory retry/audit, flash sale simulation, SKU mapping, profit and channel charts, selling price calculation <br> - Optimize the operations screens                                                                                                                                                                                                 | 06/11/2026 | 06/11/2026      | NexusOMS repository                                                   |
| 7   | - Final UI review: searchable select inside popups/modals no longer overflows the screen <br> - Adjust the layouts of tables, warehouse filters, customers, orders, inventory and pricing <br> - Check frontend build, typecheck and tests                                                                                                                                                              | 07/11/2026 | 07/11/2026      | NexusOMS repository                                                   |
| 1-7 | - Review the target AWS architecture: CloudFront, S3, WAF, ACM, ALB, ECS Fargate across two AZs, two public and two private subnets, NAT Gateway, ECR, Secrets Manager, CloudWatch, CloudTrail, VPC endpoints and Supabase Cloud <br> - Write and update the proposal in English and Vietnamese, keeping the workshop template with placeholders for the diagram, AWS Pricing Calculator, costs and ROI | 01/11/2026 | 07/11/2026      | NexusOMS proposal                                                     |
 
 
### Week 8 Achievements:
 
* Reduced the scope and changed the access model:
  * Cut the features and pages that do not serve the MVP
  * Moved from multi-tenant to a single organization with 3 roles: admin, manager and warehouse
  * Kept Supabase for Auth and PostgreSQL
* Built the backend foundation:
  * Created a new Supabase project with a baseline migration, and fixed a migration that duplicated a column
  * Connected the .NET backend to Supabase PostgreSQL and Supabase JWT
  * Set up the backend structure, Docker and basic CI/CD, and checked that secrets are excluded from Git
  * Completed the Brand, Product, Warehouse and Inventory (bins/channels) APIs
* Tested the APIs with Postman: login and access token, Brand, Product, Warehouse, Inventory, Orders, Channel connection, SKU mapping and webhooks.
* Completed the inventory logic: receive stock, adjust, reserve, release and commit, while preventing negative stock or reducing stock below the reserved quantity.
* Completed the order logic: create orders for walk-in or existing customers, status history, cancel, stock export, delivery and order profit, supported by transactions, idempotency, migrations and tests.
* Built the webhook simulator for marketplace demos with staged events: create order → reserve stock → confirm → prepare → export/ship → delivered, plus inventory retry/audit and a flash sale simulation.
* Redesigned the frontend around real business rules and connected it to the real API:
  * Marketplace orders only receive status changes from webhooks
  * Product keeps full CRUD, and customers are created from orders
  * Stock actions moved into short popups
  * API calls centralized in a service folder, with consistent state, selection, search and filter handling
  * Recharts charts on Overview, Order Profit and Channel Performance
  * Redesigned login and home pages, logo and favicon, popups and searchable dropdowns
  * Ran the frontend build, typecheck and tests during the final UI review
* Reviewed the target AWS architecture and updated the proposal documents in English and Vietnamese, keeping placeholders for the diagram, AWS Pricing Calculator, costs and ROI.
* Result: the MVP now has an end-to-end demo flow from products, warehouses and SKU mapping to order webhooks, stock reservation, stock export and profit reports. The AWS part is still the target deployment architecture and not a continuously running production infrastructure.