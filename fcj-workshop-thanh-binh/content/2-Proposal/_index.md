---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

This section summarizes the NexusOMS workshop proposal and the AWS demonstration that the team **plans** to conduct.

# NexusOMS - Multi-Channel Order and Inventory Operations Platform
## A Practical AWS Deployment for Reliable Order, Inventory and Profit Operations

### 1. Executive Summary
NexusOMS is built for a single organization that sells through Shopee, TikTok Shop, Lazada and Website D2C. It centralizes products, warehouses, inventory, marketplace orders, SKU mappings and order profitability. The implemented MVP uses a React/Vite frontend, a .NET 10 Modular Monolith API and Supabase for PostgreSQL and authentication. Access is restricted to the `admin`, `manager` and `warehouse` roles.

The system focuses on operational safety rather than ordinary CRUD. It supports atomic stock reservation, idempotent requests, signed simulated webhooks, retry processing, inventory transactions and CM1–CM3 profitability. Real marketplace accounts are not available, so the team uses a simulator with payload structures inspired by marketplace APIs to demonstrate the order lifecycle.

### 2. Problem Statement
### What’s the Problem?
Multi-channel sellers frequently switch between marketplace dashboards to manage orders and stock. Concurrent flash-sale orders may oversell a SKU, repeated webhook delivery may create duplicate records, and profit is difficult to understand after product cost, payment fees, fulfillment cost and advertising.

### The Solution
NexusOMS receives a simulated marketplace webhook, verifies its HMAC signature, maps the channel SKU to an internal product and reserves available inventory in a database transaction. The order can then move through confirmation, preparation, shipment and delivery. Idempotency keys and database constraints prevent duplicate delivery from generating a second business operation. The frontend exposes the operational flow, warehouse balances, channel mappings and CM1–CM3 per-order/channel reporting.

### Benefits and Return on Investment
The platform creates one operating view for products, stock and orders, reducing manual reconciliation between channels. Reservation and commit/release operations make flash-sale behavior demonstrable and auditable. The architecture can be deployed for a short AWS demonstration, photographed for the report and removed afterwards. **PLACEHOLDER - add the team’s business assumptions, measured time savings and ROI calculation.**

### 3. Solution Architecture
The current application runs locally with React/Vite, a .NET 10 API and Supabase Cloud. The target demonstration architecture serves the frontend from Amazon S3 through CloudFront, sends API requests through an Application Load Balancer, and runs the API as an Amazon ECS Fargate service in two private subnets across two Availability Zones. NAT Gateways provide outbound access from the tasks to Supabase Cloud and simulated marketplace endpoints. The architecture uses two public and two private subnets, with WAF and ACM at the public edge.

![NexusOMS business flow architecture](/images/2-Proposal/noxus-aws-architect-1.png)

### AWS Services Used
- **Amazon S3 and Amazon CloudFront**: Store and distribute the static React/Vite build.
- **AWS WAF and AWS Certificate Manager**: Protect public traffic and provide TLS certificates.
- **Application Load Balancer**: Routes HTTPS API traffic to healthy ECS tasks.
- **Amazon ECS on AWS Fargate**: Runs the containerized .NET 10 API across two Availability Zones.
- **Amazon ECR**: Stores versioned backend container images built from GitHub CI.
- **Amazon VPC, NAT Gateway and Internet Gateway**: Isolate API tasks in private subnets while allowing controlled outbound access.
- **VPC Endpoints, AWS Secrets Manager and Amazon CloudWatch**: Retrieve container images/secrets privately where applicable and collect logs/metrics.
- **AWS CloudTrail and Amazon S3**: Record AWS API activity and retain access/load-balancer logs.
- **Supabase Cloud**: Remains the external PostgreSQL database and authentication provider; it is not replaced with Amazon RDS in this MVP.

### Component Design
- **Frontend**: React/Vite presents product, warehouse, order, profit and webhook-simulation workflows.
- **API**: .NET 10 Modular Monolith exposes REST endpoints, validates Supabase JWTs and applies role policies.
- **Order and inventory workflow**: PostgreSQL transactions reserve, release or commit stock while preserving an inventory-operation record.
- **Webhook workflow**: HMAC verification, idempotency and retry processing protect marketplace-style simulated events.
- **Database and identity**: Supabase provides PostgreSQL and Auth; the database uses a single-organization `public` schema.
- **AWS runtime**: ECS tasks obtain secrets safely, send logs to CloudWatch and reach Supabase/marketplace simulators through NAT Gateways.

### 4. Technical Implementation
**Implementation Phases**
This project has two parts-completing the NexusOMS MVP and preparing a short AWS demonstration-each following 4 phases:
- Build Theory and Draw Architecture: review the order/inventory domain, inspect the existing codebase and draw the target AWS architecture.
- Calculate Price and Check Practicality: use AWS Pricing Calculator to estimate the selected-region, short-duration demonstration cost and adjust if needed.
- Fix Architecture for Cost or Solution Fit: review security groups, IAM permissions, NAT usage, image delivery and cleanup scope before deployment.
- Develop, Test, and Deploy: test frontend/backend flows locally, build the API container, deploy the selected AWS services, verify logs and remove temporary resources after the demonstration.

**Technical Requirements**
- NexusOMS Application: React/Vite frontend, .NET 10 API, PostgreSQL/Npgsql, Supabase Auth JWT verification, role policies and HMAC webhook validation.
- AWS Demonstration: Docker image in ECR, ECS Fargate service behind ALB, S3/CloudFront frontend, two public plus two private subnets, NAT Gateways, CloudWatch logs, Secrets Manager and AWS IAM roles. Supabase environment values are stored as secrets, not committed to source control.

### 5. Timeline & Milestones
**Project Timeline**
- Preparation: finalize source-code flows, test cases, architecture diagram and AWS cost worksheet.
- Demonstration setup:
    - Configure the network, IAM roles, ECR, S3/CloudFront and observability resources.
    - Deploy the container service and configure ALB, TLS and secrets.
    - Run the webhook/order/inventory demonstration and capture report evidence.
- Post-Demonstration: delete temporary AWS resources and retain only code, diagrams, screenshots and cost evidence for the report.

### 6. Budget Estimation
The estimate was prepared in AWS Pricing Calculator for **Asia Pacific (Singapore) / ap-southeast-1**. The values below are the monthly costs for the demo configuration and usage entered in the Calculator. The actual 10-day bill may differ based on each resource’s runtime, actual traffic, Free Tier eligibility and taxes.

### Infrastructure Costs
| Service | Estimate (USD/month) |
| --- | ---: |
| Amazon ECS Fargate | 3.75 |
| Application Load Balancer | 18.98 |
| 2 NAT Gateways | 86.14 |
| VPC Interface Endpoints | 113.88 |
| Amazon S3 | 0.02 |
| Amazon ECR | 0.20 |
| Amazon CloudWatch | 0.07 |
| AWS Secrets Manager | 0.13 |
| Amazon CloudTrail | 0.00 |
| Amazon CloudFront | 0.00 |
| AWS WAF (separate estimate; disabled for this demo) | 14.00 |
| **Total including WAF** | **237.17** |
| **Total with WAF disabled** | **223.17** |

The Calculator reports **$0.00 upfront cost**. WAF is included in the budget to show the protected option, but it will remain disabled during this demonstration. NAT Gateways, the ALB and VPC Interface Endpoints are primarily hourly charges, so the monthly total is not an exact 10-day invoice. A simple 10/30-day proration gives a reference estimate of approximately **$79.06 including WAF** (before tax); the actual charge should be checked in AWS Billing after resources are deleted.

- Hardware: No additional hardware is required; the project uses the existing development machines and Supabase Cloud project.

### 7. Risk Assessment
#### Risk Matrix
- Marketplace credentials unavailable: Medium impact, high probability.
- Stock conflict or duplicate webhook: High impact, medium probability.
- AWS cost overrun during the demonstration: Medium impact, low probability.

#### Mitigation Strategies
- Marketplace integration: use the HMAC-protected simulator and documented representative payloads.
- Order/inventory integrity: use idempotency keys, database constraints and reserve/release/commit transactions.
- Cost: create AWS Budgets/billing alerts, constrain the demo duration and delete chargeable resources afterwards.

#### Contingency Plans
- Run the complete functional demonstration locally with Supabase if an AWS deployment service is unavailable.
- Keep infrastructure definitions and the architecture diagram so the demonstration environment can be recreated and removed predictably.

### 8. Expected Outcomes
#### Technical Improvements: 
One system demonstrates product/SKU mapping, warehouse inventory, order lifecycle, webhook safety and profitability instead of isolated manual workflows.
#### Long-term Value
The MVP and AWS architecture form a reusable foundation for real marketplace credentials, production webhook adapters and future reporting improvements.
