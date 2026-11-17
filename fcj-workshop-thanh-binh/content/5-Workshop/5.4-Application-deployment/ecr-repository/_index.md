---
title: "ECR repository"
date: 2026-11-17
weight: 1
chapter: false
---

## 1. Open Amazon ECR

Select **Asia Pacific (Singapore) / ap-southeast-1**.

Open **Amazon ECR → Private registry → Repositories → Create repository**.

![Open ECR and create a repository](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/1-open-ecr-dashboard-and-create-repository.png)

## 2. Configure the Repository

Enter:

```text
Visibility: Private
Repository name: nexusoms-demo-api
Image tag mutability: Immutable
Scan on push: Enabled
Encryption: AES-256
```

![Configure the private repository](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/2-configure-private-repository.png)

## 3. Create the Repository

Review the configuration and choose **Create repository**.

![Review encryption and create the repository](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/3-confirm-encryption-and-create-repository.png)

## 4. Copy the Repository URI

Open `nexusoms-demo-api` and choose **Copy URI**.

![Copy the ECR repository URI](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/4-copy-ecr-repository-uri.png)

Save the URI for the backend image build and push steps:

```text
<ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/nexusoms-demo-api
```
