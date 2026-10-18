---
title: "Week 5 Worklog"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

- Complete the final lab series of the FCJ learning roadmap (Document Management System - Serverless).
- Deploy a full-stack application from local to AWS end-to-end using CLI tools.
- Begin the OJT Workshop project phase: study C# foundations and plan the NexusOMS project.

### Tasks to be carried out this week:

| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Start Date | Completion Date | Reference Material                                                                                                                                                                                                                                                                                                                                         |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | - **Practice - Serverless Document Management System Series (Part 1):** <br>&emsp; + Build CRUD Serverless with Lambda and DynamoDB <br>&emsp; + File storage and authentication with AWS Amplify <br>&emsp; + Integrate Frontend with API Gateway <br>&emsp; + Deploy Document Management System with AWS SAM <br>&emsp; + Set up content distribution with CloudFront <br>&emsp; + Add search functionality <br>&emsp; + DevOps for Document Management System <br>&emsp; + Distributed tracing with X-Ray and CloudWatch                                                                                                                                                                                                                                   | 12/10/2026 | 12/10/2026      | <https://000133.awsstudygroup.com/vi/> <br> <https://000134.awsstudygroup.com/vi/> <br> <https://000135.awsstudygroup.com/vi/> <br> <https://000136.awsstudygroup.com/vi/> <br> <https://000137.awsstudygroup.com/vi/> <br> <https://000138.awsstudygroup.com/vi/> <br> <https://000139.awsstudygroup.com/vi/> <br> <https://000140.awsstudygroup.com/vi/> |
| 2   | - **Practice - Serverless Document Management System Series (Part 2 - Completed):** <br>&emsp; + Full end-to-end deployment from local to AWS via PowerShell and Ubuntu (WSL2) <br>&emsp; + Resolved Cloud9 restriction by running all commands locally <br>&emsp; + Self-troubleshot outdated library versions: identified and upgraded dependencies <br>&emsp; + Workshop Serverless API (000066) <br>&emsp; + CI/CD on EKS with CodePipeline and GitHub (000062) - reached 90% completion <br> - **Completed the entire FCJ lab learning roadmap**                                                                                                                                                                                                      | 13/10/2026 | 13/10/2026      | <https://000133.awsstudygroup.com/vi/> <br> <https://000066.awsstudygroup.com/vi/> <br> <https://000062.awsstudygroup.com/vi/> <br> PowerShell, Ubuntu (WSL2) <br> AWS CLI, AWS Amplify CLI, AWS SAM CLI                                                                                                                                                   |
| 3-5 | - Study C# fundamentals via Coursera (FPT University): <br>&emsp; + Classes, interfaces, LINQ, async/await basics <br>&emsp; + Foundation required before starting the .NET project <br> - Plan the OJT Workshop project: **NexusOMS** <br>&emsp; + High-Concurrency Multi-Channel Order & Inventory Hub (.NET + PostgreSQL + AWS) <br>&emsp; + Market research: analyzed existing OMS products (Sapo, Shopify, TikTok Shop) <br>&emsp; + Defined 4 core business problems: overselling prevention, multi-channel inventory sync, idempotency, PII security <br>&emsp; + Scoped 18-week plan across 3 phases with Core / Stretch / Cut classification <br>&emsp; + Identified technical constraints upfront (no real Partner API access, FsCheck limitations) | 14/10/2026 | 16/10/2026      | <https://www.coursera.org/organizations/fptuniversity/learn/c-sharp-programming-foundations> <br> NexusOMS 18-week plan (consolidated, reviewed and revised) <br> Sapo Omnichannel, Shopify Inventory API, TikTok Shop Partner Center Docs <br> Reference: kgrzybek/modular-monolith-with-ddd, dotnet/eShop, Milan Jovanović                               |

### Week 5 Achievements:

- **Completed the full FCJ Workforce lab roadmap** - a significant milestone after 5 weeks of continuous hands-on practice across 60+ labs.
- Built a complete serverless Document Management System running entirely on AWS:
  - Backend: Lambda + DynamoDB + S3 (full CRUD)
  - Authentication: AWS Amplify
  - API layer: API Gateway
  - Infrastructure as Code: AWS SAM (YAML)
  - Content delivery: CloudFront
  - Search functionality integrated
  - Distributed tracing: X-Ray + CloudWatch
- Demonstrated strong CLI proficiency by deploying the full application end-to-end from local machine:
  - Adapted workflow when Cloud9 was restricted - switched to PowerShell + WSL2 without losing progress
  - Self-resolved all issues: outdated library dependencies, IAM configuration, API Gateway errors
  - Used CloudWatch logs effectively to identify and debug Lambda errors
- Completed CI/CD pipeline on EKS with CodePipeline and GitHub (90%) - limited by CodeBuild Quota on Free Tier account.
- Transitioned to the OJT Workshop project phase and completed initial planning for **NexusOMS**:
  - Conducted market research on existing OMS products to define realistic feature scope
  - Applied the "depth over breadth" principle: prioritized Core features to ensure thorough understanding rather than surface-level coverage
  - Selected Modular Monolith architecture (to be documented as ADR) over pure Microservices - appropriate for learning stage and avoids premature decomposition
  - Defined clear technical boundaries upfront (mock Partner APIs, FsCheck model limitations) to ensure honest project presentation during interviews
  - Each technology in the stack must answer 4 questions: how it works internally, when it fails, what the trade-offs are, and how to measure it
- Started C# fundamentals to build the necessary foundation before tackling advanced .NET topics (Outbox, Kafka, Saga).
