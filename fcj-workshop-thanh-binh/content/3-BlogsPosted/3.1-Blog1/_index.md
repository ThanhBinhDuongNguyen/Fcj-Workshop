---
title: "Deploying from GitHub Actions to AWS without Access Keys"
date: 2026-11-24
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# DEPLOYING FROM GITHUB ACTIONS TO AWS WITHOUT ACCESS KEYS: HOW DOES OIDC WORK?

While building the CI/CD pipeline for NexusOMS, the team used GitHub Actions to deploy the backend to Amazon ECR and Amazon ECS, and the frontend to Amazon S3 and Amazon CloudFront. The workflow requires permission to call AWS APIs without storing long-lived AWS access keys in GitHub Secrets.

The solution uses **OpenID Connect (OIDC)** together with **AWS Security Token Service (AWS STS)**.

### Authentication flow

1. GitHub Actions requests an OIDC token for the running workflow.
2. The token contains claims such as `aud`, `sub`, repository and branch.
3. The workflow calls `AssumeRoleWithWebIdentity` to request access from AWS STS.
4. AWS validates the token against the IAM role trust policy.
5. If the conditions match, AWS STS returns temporary credentials to the workflow.
6. The workflow uses the role permission policy to access ECR, ECS, S3 and CloudFront.

### Key points

- Long-lived access keys are not stored in GitHub Secrets.
- Credentials are short-lived and issued separately for each workflow run.
- The trust policy must restrict the correct repository, branch and audience.
- The permission policy should grant only the permissions required for deployment.
- OIDC determines which workflow may assume the role; the permission policy determines which AWS resources the role may access.

![DEPLOYING FROM GITHUB ACTIONS TO AWS WITHOUT ACCESS KEYS](/images/3-Blog/blog1.png)
The Vietnamese article was approved and published in AWS Study Group:
[View the post on Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2346635502768075)
