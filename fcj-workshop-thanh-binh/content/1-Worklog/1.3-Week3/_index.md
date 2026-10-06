---
title: "Week 3 Worklog"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 3 Objectives:

* Review and consolidate knowledge of AWS services studied in previous weeks.
* Deepen understanding of advanced AWS security, monitoring, and networking services.
* Analyze real-world architecture diagrams from the FCJ community to prepare for the Workshop project.
* Practice hands-on labs on automation, observability, and network connectivity.

### Tasks to be carried out this week:

| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Start Date | Completion Date | Reference Material                                                                                                                                                                                                                                                 |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1   | - Review all AWS services learned so far (Compute, Storage, Networking, Database, Security) <br> - Analyze FCJ community Architecture Diagrams to evaluate service frequency and relevance for Workshop <br> - Research FCJ Workforce project requirements and scoring criteria                                                                                                                                                                                                                                                                                      | 28/09/2026 | 28/09/2026      | <https://cloudjourney.awsstudygroup.com/> <br> <https://hcm-rules.awsfcaj.com/3-project/> <br> <https://github.com/thienluhoan/fcj-workshop-template>                                                                                                              |
| 2   | - **Practice:** <br>&emsp; + Lambda Automation: auto stop/start EC2 instances by schedule to optimize cost <br>&emsp; + Advanced CloudWatch + Grafana: integrate Grafana dashboard with CloudWatch metrics <br>&emsp; + Advanced CloudWatch Workshop: custom metrics, log insights, composite alarms <br>&emsp; + Tags & Resource Groups: standardize tagging strategy for AWS resources <br>&emsp; + IAM + Resource Tags (ABAC): control access based on resource tags <br>&emsp; + AWS Systems Manager: Run Command, Session Manager, Patch Manager                | 29/09/2026 | 29/09/2026      | <https://000022.awsstudygroup.com/vi/> <br> <https://000029.awsstudygroup.com/vi/> <br> <https://000036.awsstudygroup.com/vi/> <br> <https://000027.awsstudygroup.com/vi/> <br> <https://000028.awsstudygroup.com/vi/> <br> <https://000031.awsstudygroup.com/vi/> |
| 3   | - **Practice:** <br>&emsp; + Amazon SSO (IAM Identity Center): Single Sign-On setup for AWS Organization (read only — requires account upgrade) <br>&emsp; + IAM Permission Boundary: limit maximum permissions to prevent privilege escalation <br>&emsp; + IAM Role Chaining with Condition: restrict role assumption by IP, time, MFA conditions <br>&emsp; + AWS Security Hub: automated security standards assessment (CIS, PCI DSS, AWS Foundational) <br>&emsp; + AWS WAF: protect web applications and APIs against SQLi, XSS, DDoS Layer 7                  | 01/10/2026 | 01/10/2026      | <https://000012.awsstudygroup.com/vi/> <br> <https://000030.awsstudygroup.com/vi/> <br> <https://000044.awsstudygroup.com/vi/> <br> <https://000018.awsstudygroup.com/vi/> <br> <https://000026.awsstudygroup.com/vi/>                                             |
| 4   | - **Practice:** <br>&emsp; + AWS KMS: create and manage encryption keys, integrate with S3/EBS/RDS <br>&emsp; + AWS Backup: set up automated backup plans with retention policy across AWS resources <br>&emsp; + VPC Peering: directly connect two VPCs for private IP communication <br>&emsp; + AWS Transit Gateway: centralize management of multiple VPC connections (hub-and-spoke) <br> - Created new AWS account after previous account token expired unexpectedly                                                                                           | 03/10/2026 | 03/10/2026      | <https://000033.awsstudygroup.com/vi/> <br> <https://000013.awsstudygroup.com/vi/> <br> <https://000019.awsstudygroup.com/vi/> <br> <https://000020.awsstudygroup.com/vi/>                                                                                         |
| 5   | - Read and analyze (no hands-on due to Free Tier limitations): <br>&emsp; + AWS File Storage Gateway: extend on-premises storage to S3 via NFS/SMB <br>&emsp; + Amazon FSx for Windows: shared file storage for Windows workloads <br>&emsp; + Data Lake on AWS: S3 → Glue Crawler → Data Catalog → Athena query pattern <br> - **Practice:** <br>&emsp; + Amazon DynamoDB Advanced: Global Tables, DynamoDB Streams, DAX, TTL, backup & restore <br> - Self-read architecture diagrams and independently provisioned resources before cross-checking with lab guide | 04/10/2026 | 04/10/2026      | <https://000024.awsstudygroup.com/vi/> <br> <https://000025.awsstudygroup.com/vi/> <br> <https://000035.awsstudygroup.com/vi/> <br> <https://000039.awsstudygroup.com/vi/>                                                                                         |


### Week 3 Achievements:

* Consolidated understanding of all major AWS service categories studied so far and identified the most critical services for the final Workshop project.

* Mastered advanced monitoring and automation patterns:
  * Used Lambda to automatically stop/start EC2 instances on schedule — practical cost optimization technique
  * Built Grafana dashboards integrated with CloudWatch for enhanced observability
  * Created custom metrics, log insights queries, and composite alarms in CloudWatch

* Deepened AWS security knowledge across multiple layers:
  * Outer layer: AWS WAF blocking SQLi, XSS, rate-based attacks
  * Network layer: Security Groups and NACLs
  * Identity layer: IAM Permission Boundary and Condition-based Role Chaining (ABAC)
  * Compliance layer: AWS Security Hub automated assessment against CIS and PCI DSS standards

* Understood advanced networking connectivity patterns:
  * VPC Peering for direct point-to-point VPC communication
  * AWS Transit Gateway as a central hub for managing multiple VPC connections at scale
  * Key insight: Transit Gateway is more scalable than VPC Peering when managing more than a few VPCs

* Applied standardized tagging strategy (Project, Environment, Owner) for resource management and cost allocation.

* Analyzed 8 real-world Architecture Diagrams from the FCJ community and identified the most common patterns:
  * S3 + CloudFront (frontend delivery)
  * Lambda + API Gateway (serverless backend)
  * ECS Fargate + RDS (containerized application)

* Demonstrated significant skill progression: able to independently read Architecture Diagrams and provision AWS resources from scratch before cross-checking with the lab guide — a marked improvement from Week 1.

* Successfully recovered from unexpected AWS account token expiration by creating a new account and reclaiming the $200 credit bonus.