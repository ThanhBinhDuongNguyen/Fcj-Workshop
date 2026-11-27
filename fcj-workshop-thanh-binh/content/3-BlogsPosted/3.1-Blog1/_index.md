---
title: "Blog 1"
date: 2026-11-24
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# DEPLOYING FROM GITHUB ACTIONS TO AWS WITHOUT ACCESS KEYS: HOW DOES OIDC WORK?

### Introduction

NexusOMS uses GitHub Actions to deploy the backend to ECR/ECS and the frontend to S3/CloudFront. This article explains how OpenID Connect (OIDC) and AWS STS give workflow jobs temporary credentials instead of storing long-lived access keys in GitHub Secrets.

### Key points

1. **Request a token:** The workflow obtains a GitHub OIDC token identifying the job, with claims such as aud and sub.
2. **Assume the role:** The workflow presents the token to AWS STS through AssumeRoleWithWebIdentity.
3. **Validate conditions:** AWS checks the token and the IAM role trust policy. Conditions must restrict the repository, branch and audience using the repository's actual OIDC configuration.
4. **Issue credentials:** STS returns temporary credentials for the job to call AWS APIs when the checks succeed.
5. **Limit operations:** The role's permission policy defines deployment access to ECR, ECS, S3 and CloudFront.

### Lessons learned

The trust policy determines which workflow may assume the role; the permission policy determines what the role may do. Both must be checked when a pipeline returns AccessDenied, rather than creating long-lived access keys or broadening permissions just to make it run.

### References

- [OpenID Connect in GitHub Actions](https://docs.github.com/en/actions/concepts/security/openid-connect)
- [Configuring OIDC with AWS](https://docs.github.com/en/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-aws)

### Published article and evidence

The Vietnamese article was approved and published in AWS Study Group.

[View the post on Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2346635502768075)

![Evidence of Blog 1 published in AWS Study Group](/images/3-Blog/blog1.png)
