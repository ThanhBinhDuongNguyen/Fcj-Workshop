---
title: "Private route tables"
date: 2026-11-20
weight: 6
chapter: false
---

Create one route table for each private subnet and route through the NAT Gateway in the same Availability Zone.

## Step 1: Create route table A

Open **VPC → Route tables → Create route table**.

![Open the route table creation page](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/1-open-route-tables-dashboard-click-create-route-table.png)

```text
Name: nexusoms-demo-private-a-rt
VPC: nexusoms-demo-vpc
```

![Create private route table A](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/2-fill-name-choose-vpc-create-route-table.png)

## Step 2: Add the NAT A route

Open the route table → **Routes → Edit routes**.

![Open Edit routes](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/3-choose-routes-edit-routes.png)

```text
Destination: 0.0.0.0/0
Target: NAT Gateway → nexusoms-demo-nat-a
```

![Add the NAT A route](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/4-add-route-configure-nat-gateway.png)

## Step 3: Associate private subnet A

Choose **Subnet associations → Edit subnet associations**.

![Open subnet associations](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/5-choose-subnet-associations-edit-subnet-associations.png)

Select only `nexusoms-demo-private-a`, then save.

![Associate private subnet A](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/6-choose-subnet-private-a-save-associations.png)

## Step 4: Create route table B

Repeat with:

```text
Name: nexusoms-demo-private-b-rt
Destination: 0.0.0.0/0
Target: NAT Gateway → nexusoms-demo-nat-b
Subnet association: nexusoms-demo-private-b
```

![Complete both private route tables](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/7-complete-create-private-route-table.png)

## Step 5: Verify

- Private A routes through NAT A.
- Private B routes through NAT B.
- Neither private route table points directly to the Internet Gateway.
