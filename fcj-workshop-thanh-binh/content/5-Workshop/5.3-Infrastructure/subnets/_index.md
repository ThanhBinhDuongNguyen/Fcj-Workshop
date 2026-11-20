---
title: "Create public and private subnets"
date: 2026-11-20
weight: 2
chapter: false
---

Create four subnets across two Availability Zones.

## Step 1: Open the subnet creation page

Open **VPC → Subnets → Create subnet**.

![Open the subnet creation page](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/1-open-subnets-dashboard-create-subnet.png)

Select:

```text
nexusoms-demo-vpc
```

![Select the VPC](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/2-choose-vpc-id.png)

## Step 2: Create public subnet A

```text
Name: nexusoms-demo-public-a
Availability Zone: AZ A
IPv4 CIDR: 10.80.0.0/24
```

![Configure public subnet A](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/3-configure-public-subnet-a.png)

## Step 3: Create public subnet B

```text
Name: nexusoms-demo-public-b
Availability Zone: AZ B
IPv4 CIDR: 10.80.1.0/24
```

![Configure public subnet B](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/4-configure-public-subnet-b.png)

## Step 4: Create private subnet A

```text
Name: nexusoms-demo-private-a
Availability Zone: same AZ as public-a
IPv4 CIDR: 10.80.10.0/24
```

![Configure private subnet A](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/5-configure-private-subnet-a.png)

## Step 5: Create private subnet B

```text
Name: nexusoms-demo-private-b
Availability Zone: same AZ as public-b
IPv4 CIDR: 10.80.11.0/24
```

![Configure private subnet B](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/6-configure-private-subnet-b.png)

Choose **Create subnet**.

## Step 6: Enable public IPv4 on public subnets

Select `nexusoms-demo-public-a` → **Actions → Edit subnet settings**.

![Open subnet settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/7-edit-subnet-settings.png)

Enable **Enable auto-assign public IPv4 address** and save.

![Enable auto-assign public IPv4](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/8-enable-auto-assign-public-ipv4-address.png)

Repeat for `nexusoms-demo-public-b`. Keep this setting disabled on both private subnets.

## Step 7: Verify

| Subnet | CIDR | Public IPv4 |
| --- | --- | --- |
| `nexusoms-demo-public-a` | `10.80.0.0/24` | Enabled |
| `nexusoms-demo-public-b` | `10.80.1.0/24` | Enabled |
| `nexusoms-demo-private-a` | `10.80.10.0/24` | Disabled |
| `nexusoms-demo-private-b` | `10.80.11.0/24` | Disabled |
