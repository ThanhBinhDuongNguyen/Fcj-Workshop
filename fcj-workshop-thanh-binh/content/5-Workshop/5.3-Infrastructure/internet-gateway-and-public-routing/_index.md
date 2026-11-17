---
title: "Internet gateway and public routing"
date: 2026-11-13
weight: 3
chapter: false
---

Attach an internet gateway to the VPC and use one public route table for both public subnets.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**Internet gateway and public routing - input 1**

```text
nexusoms-demo-igw
```

**Public subnet value**

```text
nexusoms-demo-public-rt
```

**IPv4 CIDR**

```text
0.0.0.0/0
```

## AWS Console walkthrough

### Step 1 - Open IGW dashboard + Create IGW


![Open IGW dashboard + Create IGW](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/1-open-igw-dashboard-create-igw.png)

### Step 2 - Put name tag + Create IGW

![Put name tag + Create IGW](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/2-put-name-tag-create-igw.png)

### Step 3 - Attach to a VPC

![Attach to a VPC](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/3-attach-to-a-vpc.png)

### Step 4 - Choose VPC + Click Attach IGW

![Choose VPC + Click Attach IGW](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/4-choose-vpc-click-attach-igw.png)

### Step 5 - Open Route tables dashboard + Create Route table

![Open Route tables dashboard + Create Route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/5-open-route-tables-dashboard-create-route-table.png)

### Step 6 - Fill Name + Choose VPC + Create route table

**Values used in this step**

**IPv4 CIDR**

```text
0.0.0.0/0
```

![Fill Name + Choose VPC + Create route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/6-fill-name-choose-vpc-create-route-table.png)

### Step 7 - Choose Routes + Edit routes

![Choose Routes + Edit routes](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/7-choose-routes-edit-routes.png)

### Step 8 - Add route + choose Internet gateway + Save changes

**Values used in this step**

**Internet gateway and public routing - input 1**

```text
nexusoms-demo-igw
```

![Add route + choose Internet gateway + Save changes](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/8-add-route-choose-internet-gateway-save-changes.png)

### Step 9 - Choose subnet associations + Edit subnet associations

![Choose subnet associations + Edit subnet associations](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/9-choose-subnet-associations-edit-subnet-associations.png)

### Step 10 - Choose 2 public subnet + Save associations

**Values used in this step**

**Public subnet value**

```text
nexusoms-demo-public-rt
```

![Choose 2 public subnet + Save associations](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/10-choose-2-public-subnet-save-associations.png)
