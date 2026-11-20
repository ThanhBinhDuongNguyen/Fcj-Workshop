---
title: "Security Groups"
date: 2026-11-20
weight: 7
chapter: false
---

Create one Security Group for the ALB and one for the ECS tasks.

## Step 1: Open the Security Group creation page

Open **VPC → Security Groups → Create security group**.

![Open the Security Group creation page](/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/1-open-security-groups-dashboard-create-security-group.png)

## Step 2: Create the ALB Security Group

```text
Name: nexusoms-demo-alb-sg
VPC: nexusoms-demo-vpc
Inbound type: HTTP
Protocol: TCP
Port: 80
Source: <CLOUDFRONT_ORIGIN_PREFIX_LIST_ID>
```

For Source, choose **Prefix list**, then select `com.amazonaws.global.cloudfront.origin-facing`.

![Configure the ALB Security Group](/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/2-full-basic-details-of-security-group.png)

## Step 3: Create the ECS task Security Group

```text
Name: nexusoms-demo-task-sg
VPC: nexusoms-demo-vpc
Inbound type: Custom TCP
Port: 8080
Source: nexusoms-demo-alb-sg
```

Do not add a public inbound rule to the ECS task.

![Configure the ECS task Security Group](/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/3-configure-the-second-security-group.png)

## Step 4: Verify

- The ALB accepts HTTP:80 from the CloudFront managed prefix list.
- ECS tasks accept TCP:8080 only from `nexusoms-demo-alb-sg`.
- Keep the default outbound rules for the lab.

Because the ALB accepts traffic only from CloudFront origin-facing networks, opening the ALB DNS name directly may time out. Verify the API through `https://<CLOUDFRONT_DOMAIN>/api/...`; do not broaden the security group or change the health check to hide a `500` response.
