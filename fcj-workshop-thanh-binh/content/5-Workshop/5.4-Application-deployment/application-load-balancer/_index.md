---
title: "Application Load Balancer"
date: 2026-11-13
weight: 4
chapter: false
---

Create an Application Load Balancer to route API requests from CloudFront to the Fargate tasks.

## Step 1: Open the Load Balancer creation page

Open **EC2 → Load Balancers → Create load balancer**.

![Open the Load Balancer creation page](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/1-open-load-balancers-dashboard-create-load-balancer.png)

Choose **Application Load Balancer → Create**.

![Choose Application Load Balancer](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/2-choose-create-application-load-balancer.png)

## Step 2: Configure the basic settings

```text
Name: nexusoms-demo-alb
Scheme: Internet-facing
IP address type: IPv4
```

![Configure the basic settings](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/3-configure-basic-load-balancer.png)

## Step 3: Select the VPC and subnets

```text
VPC: nexusoms-demo-vpc
Mappings: public-a and public-b
```

![Select the VPC and public subnets](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/4-configure-network-mapping-for-load-balancer.png)

## Step 4: Select the Security Group

Select only `nexusoms-demo-alb-sg`.

![Select the ALB Security Group](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/5-configure-security-group-for-load-balancer.png)

## Step 5: Configure the listener and target group

```text
Protocol: HTTP
Port: 80
Default action: Forward to nexusoms-demo-api-tg
```

![Select the target group](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/6-configure-target-group-for-load-balancer.png)

Choose **Create load balancer** and wait for the status to become **Active**.

## Step 6: Record the ALB information

Open the new ALB and record its **DNS name** and **Load balancer ARN**.

![Copy the ALB ARN](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/7-copy-load-balancer-arn.png)
