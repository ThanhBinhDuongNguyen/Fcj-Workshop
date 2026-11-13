---
title: "Private route tables"
date: 2026-11-13
weight: 6
chapter: false
---

Create one route table per private subnet and route outbound traffic through the NAT gateway in the same Availability Zone.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**NAT Gateway value**

```text
nexusoms-demo-private-a-rt | 0.0.0.0/0 -> nexusoms-demo-nat-a
```

**NAT Gateway value**

```text
nexusoms-demo-private-b-rt | 0.0.0.0/0 -> nexusoms-demo-nat-b
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open Route tables dashboard + Click create route table

In AWS Console, complete **Open Route tables dashboard + Click create route table** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/1. Open Route tables dashboard + Click create route table.png" title="Open Route tables dashboard + Click create route table" >}}

### Step 2 - Fill Name + Choose VPC + Create route table

In AWS Console, complete **Fill Name + Choose VPC + Create route table** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/2. Fill Name + Choose VPC + Create route table.png" title="Fill Name + Choose VPC + Create route table" >}}

### Step 3 - Choose routes + Edit routes

In AWS Console, complete **Choose routes + Edit routes** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/3. Choose routes + Edit routes.png" title="Choose routes + Edit routes" >}}

### Step 4 - Add Route + Configure NAT Gateway

In AWS Console, complete **Add Route + Configure NAT Gateway** as shown below.

**Values used in this step**

**NAT Gateway value**

```text
nexusoms-demo-private-a-rt | 0.0.0.0/0 -> nexusoms-demo-nat-a
```

**NAT Gateway value**

```text
nexusoms-demo-private-b-rt | 0.0.0.0/0 -> nexusoms-demo-nat-b
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/4. Add Route + Configure NAT Gateway.png" title="Add Route + Configure NAT Gateway" >}}

### Step 5 - Choose Subnet associations + Edit subnet associations

In AWS Console, complete **Choose Subnet associations + Edit subnet associations** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/5. Choose Subnet associations + Edit subnet associations.png" title="Choose Subnet associations + Edit subnet associations" >}}

### Step 6 - Choose subnet private a + Save associations

In AWS Console, complete **Choose subnet private a + Save associations** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/6. Choose subnet private a + Save associations.png" title="Choose subnet private a + Save associations" >}}

### Step 7 - Complete create private route table

In AWS Console, complete **Complete create private route table** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/7. Complete create private route table.png" title="Complete create private route table" >}}
