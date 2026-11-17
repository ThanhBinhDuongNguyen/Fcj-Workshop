---
title: "Private route tables"
date: 2026-11-13
weight: 6
chapter: false
---

Create one route table per private subnet and route outbound traffic through the NAT gateway in the same Availability Zone.

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

### Step 1 - Open Route tables dashboard + Click create route table

![Open Route tables dashboard + Click create route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/1-open-route-tables-dashboard-click-create-route-table.png)

### Step 2 - Fill Name + Choose VPC + Create route table

![Fill Name + Choose VPC + Create route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/2-fill-name-choose-vpc-create-route-table.png)

### Step 3 - Choose routes + Edit routes

![Choose routes + Edit routes](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/3-choose-routes-edit-routes.png)

### Step 4 - Add Route + Configure NAT Gateway

**Values used in this step**

**NAT Gateway value**

```text
nexusoms-demo-private-a-rt | 0.0.0.0/0 -> nexusoms-demo-nat-a
```

**NAT Gateway value**

```text
nexusoms-demo-private-b-rt | 0.0.0.0/0 -> nexusoms-demo-nat-b
```

![Add Route + Configure NAT Gateway](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/4-add-route-configure-nat-gateway.png)

### Step 5 - Choose Subnet associations + Edit subnet associations

![Choose Subnet associations + Edit subnet associations](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/5-choose-subnet-associations-edit-subnet-associations.png)

### Step 6 - Choose subnet private a + Save associations

![Choose subnet private a + Save associations](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/6-choose-subnet-private-a-save-associations.png)

### Step 7 - Complete create private route table

![Complete create private route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/7-complete-create-private-route-table.png)
