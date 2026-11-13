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

In AWS Console, complete **Open IGW dashboard + Create IGW** as shown below.


{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/1. Open IGW dashboard + Create IGW.png" title="Open IGW dashboard + Create IGW" >}}

### Step 2 - Put name tag + Create IGW

In AWS Console, complete **Put name tag + Create IGW** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/2. Put name tag + Create IGW.png" title="Put name tag + Create IGW" >}}

### Step 3 - Attach to a VPC

In AWS Console, complete **Attach to a VPC** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/3. Attach to a VPC.png" title="Attach to a VPC" >}}

### Step 4 - Choose VPC + Click Attach IGW

In AWS Console, complete **Choose VPC + Click Attach IGW** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/4. Choose VPC + Click Attach IGW.png" title="Choose VPC + Click Attach IGW" >}}

### Step 5 - Open Route tables dashboard + Create Route table

In AWS Console, complete **Open Route tables dashboard + Create Route table** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/5. Open Route tables dashboard + Create Route table.png" title="Open Route tables dashboard + Create Route table" >}}

### Step 6 - Fill Name + Choose VPC + Create route table

In AWS Console, complete **Fill Name + Choose VPC + Create route table** as shown below.

**Values used in this step**

**IPv4 CIDR**

```text
0.0.0.0/0
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/6. Fill Name + Choose VPC + Create route table.png" title="Fill Name + Choose VPC + Create route table" >}}

### Step 7 - Choose Routes + Edit routes

In AWS Console, complete **Choose Routes + Edit routes** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/7. Choose Routes + Edit routes.png" title="Choose Routes + Edit routes" >}}

### Step 8 - Add route + choose Internet gateway + Save changes

In AWS Console, complete **Add route + choose Internet gateway + Save changes** as shown below.

**Values used in this step**

**Internet gateway and public routing - input 1**

```text
nexusoms-demo-igw
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/8. Add route + choose Internet gateway + Save changes.png" title="Add route + choose Internet gateway + Save changes" >}}

### Step 9 - Choose subnet associations + Edit subnet associations

In AWS Console, complete **Choose subnet associations + Edit subnet associations** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/9. Choose subnet associations + Edit subnet associations.png" title="Choose subnet associations + Edit subnet associations" >}}

### Step 10 - Choose 2 public subnet + Save associations

In AWS Console, complete **Choose 2 public subnet + Save associations** as shown below.

**Values used in this step**

**Public subnet value**

```text
nexusoms-demo-public-rt
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/10. Choose 2 public subnet + Save associations.png" title="Choose 2 public subnet + Save associations" >}}
