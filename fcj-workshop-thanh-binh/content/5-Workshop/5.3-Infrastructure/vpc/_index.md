---
title: "Create the VPC"
date: 2026-11-20
weight: 1
chapter: false
---

Create the VPC for all workshop resources.

## Step 1: Select the Region and create a budget

```text
Asia Pacific (Singapore)
ap-southeast-1
```

Open **Billing and Cost Management → Budgets → Create budget**. Create a monthly cost budget with `50%`, `80%`, and `100%` alerts.

## Step 2: Open the VPC creation page

Open **VPC → Your VPCs**.

![Open Your VPCs](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/1-open-your-vpcs-dashboard.png)

Choose **Create VPC**.

![Choose Create VPC](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/2-click-create-vpc.png)

## Step 3: Configure the VPC

```text
Resources to create: VPC only
Name tag: nexusoms-demo-vpc
IPv4 CIDR: 10.80.0.0/16
IPv6 CIDR: No IPv6 CIDR block
Tenancy: Default
```

![Configure the VPC](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/3-configure-vpc-settings.png)

Choose **Create VPC**.

## Step 4: Enable DNS

Select the VPC → **Actions → Edit VPC settings**.

![Open VPC settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/4-choose-action-edit-vpc-settings.png)

Enable:

```text
Enable DNS resolution
Enable DNS hostnames
```

![Enable DNS settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/5-check-2-box-in-dns-settings.png)

## Step 5: Record the VPC ID

Copy the VPC ID for the next steps.

![Copy the VPC ID](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/6-copy-vpc-id.png)

Verify that the CIDR is `10.80.0.0/16` and both DNS settings are enabled.
