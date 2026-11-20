---
title: "Create NAT Gateways"
date: 2026-11-20
weight: 5
chapter: false
---

Create one NAT Gateway in each Availability Zone for outbound access from the private subnets.

## Step 1: Create NAT Gateway A

Open **VPC → NAT gateways → Create NAT gateway**.

![Open the NAT Gateway creation page](/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/1-open-nat-gateways-dashboard-create-nat-gateway.png)

```text
Name: nexusoms-demo-nat-a
Subnet: nexusoms-demo-public-a
Connectivity type: Public
Elastic IP allocation ID: <ALLOCATION_ID_A>
```

![Configure NAT Gateway A](/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/2-configure-nat-gateway-settings.png)

Choose **Create NAT gateway**.

![Create NAT Gateway A](/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/3-click-create-nat-gateway.png)

## Step 2: Create NAT Gateway B

Repeat with:

```text
Name: nexusoms-demo-nat-b
Subnet: nexusoms-demo-public-b
Connectivity type: Public
Elastic IP allocation ID: <ALLOCATION_ID_B>
```

## Step 3: Verify

- Both NAT Gateways are **Available**.
- NAT A is in `public-a` and uses `<ALLOCATION_ID_A>`.
- NAT B is in `public-b` and uses `<ALLOCATION_ID_B>`.

Continue only after both NAT Gateways are available.
