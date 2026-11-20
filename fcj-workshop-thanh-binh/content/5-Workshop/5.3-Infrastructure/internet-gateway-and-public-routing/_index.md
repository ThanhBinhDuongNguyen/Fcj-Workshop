---
title: "Internet Gateway and public routing"
date: 2026-11-20
weight: 3
chapter: false
---

Attach an Internet Gateway to the VPC and use one public route table for both public subnets.

## Step 1: Create the Internet Gateway

Open **VPC → Internet gateways → Create internet gateway**.

![Open the Internet Gateway creation page](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/1-open-igw-dashboard-create-igw.png)

```text
Name tag: nexusoms-demo-igw
```

Choose **Create internet gateway**.

![Create the Internet Gateway](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/2-put-name-tag-create-igw.png)

## Step 2: Attach the Internet Gateway

Choose **Actions → Attach to a VPC**.

![Choose Attach to a VPC](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/3-attach-to-a-vpc.png)

Select `nexusoms-demo-vpc` and attach.

![Attach the Internet Gateway](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/4-choose-vpc-click-attach-igw.png)

## Step 3: Create the public route table

Open **VPC → Route tables → Create route table**.

![Open the route table creation page](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/5-open-route-tables-dashboard-create-route-table.png)

```text
Name: nexusoms-demo-public-rt
VPC: nexusoms-demo-vpc
```

![Create the public route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/6-fill-name-choose-vpc-create-route-table.png)

## Step 4: Add the Internet route

Open **Routes → Edit routes**.

![Open Edit routes](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/7-choose-routes-edit-routes.png)

```text
Destination: 0.0.0.0/0
Target: Internet Gateway → nexusoms-demo-igw
```

![Add the Internet Gateway route](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/8-add-route-choose-internet-gateway-save-changes.png)

## Step 5: Associate the public subnets

Choose **Subnet associations → Edit subnet associations**.

![Open subnet associations](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/9-choose-subnet-associations-edit-subnet-associations.png)

Select both public subnets:

```text
nexusoms-demo-public-a
nexusoms-demo-public-b
```

![Associate both public subnets](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/10-choose-2-public-subnet-save-associations.png)

Verify that `0.0.0.0/0` points to `nexusoms-demo-igw` and both public subnets are associated.
