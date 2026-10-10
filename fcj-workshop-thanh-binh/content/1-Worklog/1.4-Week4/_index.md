---
title: "Week 4 Worklog"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---


### Week 4 Objectives:

* Study AWS cost optimization strategies and apply them to future project design.
* Master containerization and CI/CD pipelines using Docker, ECS, and CodePipeline.
* Explore software architecture patterns: Monolith, Microservices, and Domain-Driven Design.
* Build a complete full-stack serverless application using Lambda, DynamoDB, S3, and Cognito.

### Tasks to be carried out this week:

| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Start Date | Completion Date | Reference Material                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | - Study AWS cost optimization (read only - no hands-on): <br>&emsp; + Savings Plans (Compute vs EC2 Instance) vs Reserved Instance vs Reserved DB Instance <br>&emsp; + EC2 Resource Optimization: criteria for selecting the right instance type <br>&emsp; + Cost visualization via Cost Explorer and CloudWatch                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | 05/10/2026 | 05/10/2026      | <https://000042.awsstudygroup.com/vi/> <br> <https://000032.awsstudygroup.com/vi/> <br> <https://000034.awsstudygroup.com/vi/>                                                                                                                                                                                                                                                                                                                            |
| 2   | - **Practice:** <br>&emsp; + Deploy application with Docker: build image, push to Docker Hub/ECR via CLI, run container <br>&emsp; + Deploy on Amazon ECS: create Task Definition, Service, and Cluster <br>&emsp; + AWS CodePipeline: set up automated CI/CD pipeline (GitHub → CodeBuild → CodeDeploy) <br>&emsp; + CodePipeline Automation: end-to-end deployment automation <br> - Self-studied CodePipeline via YouTube and online resources (original lab GitHub repo was removed)                                                                                                                                                                                                                                                                                                                                 | 06/10/2026 | 06/10/2026      | <https://000015.awsstudygroup.com/vi/> <br> <https://fcj-dntu.github.io/000016-deploy-app-ecs/> <br> <https://000017.awsstudygroup.com/vi/> <br> <https://000023.awsstudygroup.com/vi/> <br> YouTube, online articles                                                                                                                                                                                                                                     |
| 3   | - Read and analyze architecture (no hands-on - lab requires Windows instance + Eclipse + Java): <br>&emsp; + Monolith to Microservices migration patterns on AWS <br>&emsp; + Automated release strategies: Blue/Green, Canary deployments <br> - Self-study via architecture books: <br>&emsp; + *Building Microservices* (Sam Newman) <br>&emsp; + *Learning Domain-Driven Design* (Vladik Khononov) <br>&emsp; + *Monolith to Microservices* (Sam Newman)                                                                                                                                                                                                                                                                                                                                                             | 07/10/2026 | 07/10/2026      | <https://000050.awsstudygroup.com/vi/> <br> <https://000051.awsstudygroup.com/vi/> <br> Building Microservices - Sam Newman <br> Learning Domain-Driven Design - Vladik Khononov <br> Monolith to Microservices - Sam Newman                                                                                                                                                                                                                              |
| 4   | - Read and analyze (no hands-on - lab requires Windows instance, template errors): <br>&emsp; + Creating a Microservice on AWS <br>&emsp; + Data restructuring and workflow in Microservice architecture <br>&emsp; + Microservice Messaging & Eventing (SQS/SNS/EventBridge) <br>&emsp; + Single Page Application authentication <br>&emsp; + AI services on AWS (Bedrock, Rekognition, Comprehend) <br>&emsp; + AWS Step Functions: serverless workflow orchestration                                                                                                                                                                                                                                                                                                                                                  | 08/10/2026 | 08/10/2026      | <https://000052.awsstudygroup.com/vi/> <br> <https://000053.awsstudygroup.com/vi/> <br> <https://000054.awsstudygroup.com/vi/> <br> <https://000055.awsstudygroup.com/vi/> <br> <https://000056.awsstudygroup.com/vi/> <br> <https://000047.awsstudygroup.com/vi/>                                                                                                                                                                                        |
| 5-6 | - **Practice (Serverless series - 9 labs):** <br>&emsp; + Serverless Backend: Lambda + S3 + DynamoDB - full CRUD API <br>&emsp; + Frontend for Serverless API - integrate React with Lambda backend <br>&emsp; + AWS SAM - Infrastructure as Code for serverless deployment <br>&emsp; + Amazon Cognito - user authentication: Login, Register, Change Password <br>&emsp; + SQS + SNS - event handling and automated email notifications <br>&emsp; + Monitoring Serverless - CloudWatch logs, metrics, X-Ray tracing <br>&emsp; + AWS AppSync + GraphQL API <br>&emsp; + CI/CD for Serverless (90% completed - source cleared prior to lab) <br> - Read only (Free Tier limitation): Custom Domain + SSL with Route 53 <br> - **Built a complete bookstore website** with full functionality using only AWS services | 09/10/2026 | 10/10/2026      | <https://000078.awsstudygroup.com/vi/> <br> <https://000079.awsstudygroup.com/vi/> <br> <https://000080.awsstudygroup.com/vi/> <br> <https://000081.awsstudygroup.com/vi/> <br> <https://000082.awsstudygroup.com/vi/> <br> <https://000083.awsstudygroup.com/vi/> <br> <https://000084.awsstudygroup.com/vi/> <br> <https://000085.awsstudygroup.com/vi/> <br> <https://000086.awsstudygroup.com/vi/> <br> <https://www.youtube.com/watch?v=z7nLsJvEyMY> |


### Week 4 Achievements:

* Understood AWS cost optimization strategies and internalized key criteria for selecting EC2 instance types in real-world scenarios:
  * CPU/RAM: estimate actual workload - t3.micro/t3.small sufficient for small web apps
  * Network: choose instances with enhanced networking for high-bandwidth needs
  * Storage: select storage-optimized instances (io1/io2) for high I/O requirements
  * Cost model: On-Demand for dev/test; Reserved/Savings Plans for stable production workloads
  * Savings Plans can reduce costs by up to 66% compared to On-Demand with a 1–3 year commitment

* Mastered containerization and CI/CD workflows:
  * Understood the full Docker workflow: write code → build image → tag → push to ECR/Docker Hub → ECS pulls and runs
  * Docker Image as an immutable artifact ensures environment consistency between dev and production
  * ECS manages the container lifecycle on AWS - eliminating the need to self-manage Docker on EC2
  * Independently studied CodePipeline through external resources after the original lab repo was removed

* Explored software architecture patterns through technical books:
  * Modular Monolith is a better starting point than jumping directly to Microservices
  * DDD helps define correct service boundaries (Bounded Context) - avoids tight coupling from incorrect decomposition
  * Strangler Fig Pattern enables safe, incremental migration from Monolith without disrupting existing systems
  * Planned to convert future labs to .NET stack (instead of Java) using AI assistance

* **Highlighted achievement: Built a complete serverless bookstore website using only AWS services:**
  * Frontend: React
  * Backend: Lambda + DynamoDB + S3
  * Authentication: Amazon Cognito (Login, Register, Change Password)
  * Notifications: Automated email via SES + Lambda
  * Independently debugged and resolved all issues: incorrect data types, invalid YAML format, environment variable configuration, Python Lambda code reading
  * Practiced Lambda triggers extensively: S3 upload → Lambda, DynamoDB Stream → Lambda sync
  * AWS SAM significantly reduced deployment time compared to manual resource creation
  * Successfully cleaned up all AWS resources after completing labs

* Demonstrated strong problem-solving skills: all issues encountered were independently researched and resolved without external assistance.

* Identified two areas to revisit in the Workshop project:
  * Custom Domain + SSL (Route 53 + ACM) - skipped due to domain purchase cost
  * CI/CD pipeline - reached 90% completion; source code was cleared before the lab, requiring re-setup