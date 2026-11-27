---
title: "Blogs Posted"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

This section lists the group's articles approved by [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj).

### [Blog 1 - DEPLOYING FROM GITHUB ACTIONS TO AWS WITHOUT ACCESS KEYS: HOW DOES OIDC WORK?](3.1-Blog1/)

This article explains how GitHub Actions uses OIDC and AWS STS to obtain temporary credentials and deploy an application to AWS without storing long-lived access keys in GitHub Secrets.

### [Blog 2 - ECS FARGATE FAILS TO START: DISTINGUISHING TASK ROLE AND EXECUTION ROLE](3.2-Blog2/)

This article analyzes an ECS Fargate startup failure caused by missing permission to retrieve a secret. It explains the responsibilities of the Task Execution Role and Task Role, and shows how to read an AccessDenied error before changing an IAM policy.

### [Blog 3 - DEPLOYING A .NET REST API ON ECS FARGATE: FROM DOCKER IMAGE TO RUNNING CONTAINER](3.3-Blog3/)

This article follows a .NET API from Docker image creation and ECR upload to an ECS Fargate deployment through GitHub Actions. It explains Task Definitions, ECS Services, ALB health checks and the configuration required to run the backend in private subnets.
