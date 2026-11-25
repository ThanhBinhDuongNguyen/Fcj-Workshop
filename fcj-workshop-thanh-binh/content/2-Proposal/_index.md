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

The system focuses on operational safety rather than ordinary CRUD. It supports atomic stock reservation, idempotent requests, signed simulated webhooks, SQS FIFO processing, inventory transactions and CM1–CM3 profitability. Real marketplace accounts are not available, so the team uses a simulator with payload structures inspired by marketplace APIs to demonstrate the order lifecycle.

### 2. Problem Statement
### What’s the Problem?
Multi-channel sellers frequently switch between marketplace dashboards to manage orders and stock. Concurrent flash-sale orders may oversell a SKU, repeated webhook delivery may create duplicate records, and profit is difficult to understand after product cost, payment fees, fulfillment cost and advertising.

### The Solution
NexusOMS receives a simulated marketplace webhook, verifies its HMAC signature, and places the valid event on an Amazon SQS FIFO queue. An ECS worker consumes the message, maps the channel SKU to an internal product, and reserves available inventory in a database transaction. The order can then move through confirmation, preparation, shipment, delivery, or cancellation. Cancelling before dispatch releases reserved stock. Idempotency keys and database constraints prevent duplicate delivery from generating a second business operation, while messages that repeatedly fail move to a dead-letter queue.

### Benefits and Return on Investment
The platform creates one operating view for products, stock and orders, reducing manual reconciliation between channels. Reservation and commit/release operations make flash-sale behavior demonstrable and auditable. This workshop validates the technical solution and does not calculate financial ROI without measured operating data.

### 3. Solution Architecture
The current application runs locally with React/Vite, a .NET 10 API and Supabase Cloud. The target demonstration architecture serves the frontend from Amazon S3 through CloudFront, sends API requests through an Application Load Balancer, and runs the API as an Amazon ECS Fargate service in two private subnets across two Availability Zones. Amazon SQS FIFO decouples webhook intake from business processing, and a dead-letter queue isolates messages that repeatedly fail. Each Availability Zone uses its own NAT Gateway so outbound connectivity remains available if one AZ fails. AWS WAF protects the application from common malicious requests. The workshop uses the default CloudFront domain and certificate, so it does not create AWS Certificate Manager resources. CloudTrail and ALB access logs are stored in one private S3 audit bucket.

![NexusOMS business flow architecture](/images/2-Proposal/noxus-aws-architect-1.png)

### Numbered Architecture Flow

| No. | Flow |
|---:|---|
| 1 | The user accesses the system through CloudFront. |
| 2 | CloudFront loads the React frontend from S3. |
| 3 | CloudFront forwards `/api/*` requests to the Application Load Balancer. |
| 4 | The ALB routes requests to healthy Fargate tasks. |
| 5 | The API validates a webhook and sends the message to SQS FIFO. |
| 6 | The background worker in Fargate polls SQS, processes messages in message-group order and deletes them after successful processing. |
| 7 | Messages that exceed the receive limit move to the dead-letter queue. |
| 8 | Fargate uses the NAT Gateway in its Availability Zone for outbound traffic. |
| 9 | The NAT Gateway routes outbound traffic through the Internet Gateway. |
| 10 | The backend connects to Supabase; the sales-platform Open API branch represents planned integration that is not implemented in the demo. |
| 11 | GitHub Actions builds and pushes the backend image to ECR. |
| 12 | GitHub Actions registers the task definition and updates the ECS service. |
| 13 | ECS deploys the new image as Fargate tasks in two Availability Zones. |
| 14 | At task startup, Fargate downloads ECR image layers from AWS-managed S3 through the S3 Gateway Endpoint. Number 14 covers the entire Fargate → Endpoint → S3 route. |
| 15 | The ALB writes access logs to the S3 audit bucket. |
| 16 | CloudTrail writes management events to the S3 audit bucket. |

The unnumbered dashed line from GitHub Actions to AWS STS represents CI/CD OIDC authentication rather than a business flow.

The two Fargate tasks are replicas of the same application in one ECS service. Each task runs the API and background worker in the same .NET process. Flows 5–6 apply to both replicas, not communication between tasks or separate services. To demonstrate the two-AZ configuration, set `Desired tasks = 2` and confirm one running task in each AZ; the lab uses one task by default.

The sales-platform Open API block represents planned integration. The demo uses the built-in Webhook Simulator to generate order creation, cancellation and burst events on demand. Order and stock changes are verified inside NexusOMS; they are not synchronized with live shops. Outbound flow 10 does not represent incoming marketplace webhooks.

### AWS Services Used
- **Amazon S3 and Amazon CloudFront**: Store and distribute the static React/Vite build.
- **AWS WAF**: Protects the application from common malicious requests.
- **Application Load Balancer**: Routes API traffic to healthy ECS tasks.
- **Amazon ECS on AWS Fargate**: Runs the containerized .NET 10 API across two Availability Zones.
- **Amazon ECR**: Stores versioned backend container images built from GitHub CI.
- **AWS STS**: Validates the GitHub OIDC token and issues temporary credentials to GitHub Actions.
- **Amazon SQS FIFO and a dead-letter queue**: Buffer webhook bursts, preserve ordering within a message group, and isolate failed messages.
- **Amazon VPC, NAT Gateway and Internet Gateway**: Isolate API tasks in private subnets while allowing controlled outbound access.
- **S3 Gateway Endpoint**: Downloads image layers from the AWS-managed S3 storage used by ECR. This is separate from the frontend and audit buckets. The backend does not currently store product images, invoices or reports in S3.
- **AWS Secrets Manager and Amazon CloudWatch**: Manage application configuration and collect logs/metrics. ECR API/registry, Secrets Manager, CloudWatch Logs and SQS use NAT Gateways in the lab; S3 traffic uses the Gateway Endpoint.
- **AWS CloudTrail and Amazon S3**: Record management events and retain CloudTrail/ALB access logs in a private audit bucket.
- **Supabase Cloud**: Provides both PostgreSQL and the Supabase Auth integration already used by the application. Keeping Supabase lets the lab focus on AWS deployment. RDS PostgreSQL can replace the database, but replacing all of Supabase also requires a separate authentication solution.

### Component Design
- **Frontend**: React/Vite presents product, warehouse, order, profit and webhook-simulation workflows.
- **API**: .NET 10 Modular Monolith exposes REST endpoints, validates Supabase JWTs and applies role policies.
- **Order and inventory workflow**: PostgreSQL transactions reserve, release or commit stock while preserving an inventory-operation record.
- **Asynchronous webhook workflow**: The API receives simulator payloads and sends them to SQS FIFO. The external webhook endpoint checks HMAC signatures; the simulator UI requires Manager/Admin access. A background worker in the same application polls the queue, processes messages and deletes them after success. The DLQ isolates failures according to the redrive policy.
- **Order cancellation workflow**: An `order.cancelled` event records the reason, changes the order state, and releases reserved inventory when the order has not been dispatched. Idempotency prevents stock from being released twice.
- **Database and identity**: Supabase provides PostgreSQL and Auth; the database uses a single-organization `public` schema.
- **AWS runtime**: The execution role lets ECS retrieve images, send logs and read secrets referenced by the task definition. The task role grants SQS access to application code. The backend reaches Supabase through NAT Gateways; the simulator runs inside the application.
- **CI/CD**: GitHub Actions uses OIDC to call AWS STS, receives temporary credentials, pushes images to ECR, updates the ECS service, syncs the frontend to S3, and creates a CloudFront invalidation.

### 4. Technical Implementation
**Implementation Phases**
This project has two parts-completing the NexusOMS MVP and preparing a short AWS demonstration-each following 4 phases:
- Build Theory and Draw Architecture: review the order/inventory domain, inspect the existing codebase and draw the target AWS architecture.
- Calculate Price and Check Practicality: use AWS Pricing Calculator to estimate the selected-region, short-duration demonstration cost and adjust if needed.
- Fix Architecture for Cost or Solution Fit: review security groups, IAM permissions, NAT usage, image delivery and cleanup scope before deployment.
- Develop, Test, and Deploy: test frontend/backend flows locally, build the API container, deploy the selected AWS services, verify logs and remove temporary resources after the demonstration.

**Technical Requirements**
- NexusOMS Application: React/Vite frontend, .NET 10 API, PostgreSQL/Npgsql, Supabase Auth JWT verification, role policies and HMAC webhook validation.
- AWS Demonstration: Docker image in ECR, ECS Fargate service behind ALB, SQS FIFO plus a DLQ for webhooks, S3/CloudFront frontend, two public plus two private subnets, NAT Gateways, CloudWatch logs, Secrets Manager and AWS IAM roles. Supabase environment values are stored as secrets, not committed to source control.

### 5. Timeline & Milestones
**Project Timeline**
- Preparation: finalize source-code flows, test cases, architecture diagram and AWS cost worksheet.
- Demonstration setup:
    - Configure the network, IAM roles, ECR, S3/CloudFront and observability resources.
    - Deploy the container service and configure ALB, CloudFront, audit logs and secrets.
    - Run webhook, order, inventory, and SQS burst demonstrations; inspect the DLQ, clean up `DEMO-*` records, and capture report evidence.
- Post-Demonstration: delete temporary AWS resources and retain only code, diagrams, screenshots and cost evidence for the report.

### 6. Budget Estimation
The estimate was prepared in AWS Pricing Calculator for **Asia Pacific (Singapore) / ap-southeast-1**. The values below are the costs for one month of the demo configuration and usage entered in the Calculator. The actual bill may differ based on traffic, Free Tier eligibility and taxes.

### Infrastructure Costs
| Service | Estimate (USD/month) |
| --- | ---: |
| Amazon ECS Fargate | 3.75 |
| Application Load Balancer | 18.98 |
| 2 NAT Gateways | 86.14 |
| Amazon S3 | 0.02 |
| Amazon ECR | 0.20 |
| Amazon CloudWatch | 0.07 |
| AWS Secrets Manager | 0.13 |
| Amazon CloudTrail | 0.00 |
| Amazon CloudFront | 0.00 |
| Amazon SQS FIFO and Dead-letter Queue | 0.05 |
| AWS WAF | 14.00 |
| **Total monthly cost** | **123.34** |

The SQS estimate assumes `100,000` billable FIFO requests per month at `$0.50 per million requests`, without applying Free Tier. The Calculator reports **$0.00 upfront cost**. The full configuration with two NAT Gateways and AWS WAF is estimated at **$123.34 per month**, before tax.

### Cost-saving option
A learning environment can use one NAT Gateway and omit AWS WAF. NAT Gateway cost then decreases to approximately **$43.07 per month**, reducing the total estimate to **$66.27 per month** and saving approximately **$57.07 per month**. This option accepts the NAT Gateway as a single point of failure and removes the application-protection layer provided by AWS WAF.

The workshop creates two CloudWatch metric alarms for unhealthy ALB targets and the SQS dead-letter queue. Monitoring cost remains grouped under Amazon CloudWatch instead of adding another service row. Keep the approved estimate and compare it with AWS Billing after the demo. The lab does not attach an SNS notification.

- Hardware: No additional hardware is required; the project uses the existing development machines and Supabase Cloud project.

### 7. Risk Assessment
#### Risk Matrix
- Marketplace credentials unavailable: Medium impact, high probability.
- Stock conflict or duplicate webhook: High impact, medium probability.
- AWS cost overrun during the demonstration: Medium impact, low probability.

#### Mitigation Strategies
- Marketplace integration: use the HMAC-protected simulator and documented representative payloads.
- Order/inventory integrity: use SQS FIFO, a DLQ, idempotency keys, database constraints and reserve/release/commit transactions.
- Cost: create AWS Budgets/billing alerts, constrain the demo duration and delete chargeable resources afterwards.

#### Contingency Plans
- Run the complete functional demonstration locally with Supabase if an AWS deployment service is unavailable.
- Keep infrastructure definitions and the architecture diagram so the demonstration environment can be recreated and removed predictably.

### 8. Expected Outcomes
#### Technical Improvements: 
One system demonstrates product/SKU mapping, warehouse inventory, an order lifecycle including cancellation, asynchronous webhook safety and profitability instead of isolated manual workflows.
#### Long-term Value
The MVP and AWS architecture form a reusable foundation for real marketplace credentials, production webhook adapters and future reporting improvements.
