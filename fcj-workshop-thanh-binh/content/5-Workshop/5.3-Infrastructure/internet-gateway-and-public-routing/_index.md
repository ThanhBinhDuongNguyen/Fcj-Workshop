---
title: "Internet gateway and public routing"
date: 2026-11-13
weight: 3
chapter: false
---

Attach an internet gateway to the VPC and use one public route table for both public subnets.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

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

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open IGW dashboard + Create IGW

Complete **Open IGW dashboard + Create IGW**.


{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/1-open-igw-dashboard-create-igw.png" title="Open IGW dashboard + Create IGW" >}}

### Step 2 - Put name tag + Create IGW

Complete **Put name tag + Create IGW**.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/2-put-name-tag-create-igw.png" title="Put name tag + Create IGW" >}}

### Step 3 - Attach to a VPC

Complete **Attach to a VPC**.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/3-attach-to-a-vpc.png" title="Attach to a VPC" >}}

### Step 4 - Choose VPC + Click Attach IGW

Complete **Choose VPC + Click Attach IGW**.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/4-choose-vpc-click-attach-igw.png" title="Choose VPC + Click Attach IGW" >}}

### Step 5 - Open Route tables dashboard + Create Route table

Complete **Open Route tables dashboard + Create Route table**.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/5-open-route-tables-dashboard-create-route-table.png" title="Open Route tables dashboard + Create Route table" >}}

### Step 6 - Fill Name + Choose VPC + Create route table

Complete **Fill Name + Choose VPC + Create route table**.

**Values used in this step**

**IPv4 CIDR**

```text
0.0.0.0/0
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/6-fill-name-choose-vpc-create-route-table.png" title="Fill Name + Choose VPC + Create route table" >}}

### Step 7 - Choose Routes + Edit routes

Complete **Choose Routes + Edit routes**.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/7-choose-routes-edit-routes.png" title="Choose Routes + Edit routes" >}}

### Step 8 - Add route + choose Internet gateway + Save changes

Complete **Add route + choose Internet gateway + Save changes**.

**Values used in this step**

**Internet gateway and public routing - input 1**

```text
nexusoms-demo-igw
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/8-add-route-choose-internet-gateway-save-changes.png" title="Add route + choose Internet gateway + Save changes" >}}

### Step 9 - Choose subnet associations + Edit subnet associations

Complete **Choose subnet associations + Edit subnet associations**.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/9-choose-subnet-associations-edit-subnet-associations.png" title="Choose subnet associations + Edit subnet associations" >}}

### Step 10 - Choose 2 public subnet + Save associations

Complete **Choose 2 public subnet + Save associations**.

**Values used in this step**

**Public subnet value**

```text
nexusoms-demo-public-rt
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/10-choose-2-public-subnet-save-associations.png" title="Choose 2 public subnet + Save associations" >}}
