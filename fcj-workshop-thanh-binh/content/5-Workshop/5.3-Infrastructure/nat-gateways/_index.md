---
title: "Create NAT gateways"
date: 2026-11-13
weight: 5
chapter: false
---

Each Availability Zone receives its own NAT gateway so private ECS tasks can reach Supabase and external webhook endpoints.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**NAT Gateway value**

```text
nexusoms-demo-nat-a | nexusoms-demo-public-a | nexusoms-demo-nat-eip-a
```

**NAT Gateway value**

```text
nexusoms-demo-nat-b | nexusoms-demo-public-b | nexusoms-demo-nat-eip-b
```

## AWS Console walkthrough

### Step 1 - Open NAT Gateways dashboard + Create NAT Gateway


![Open NAT Gateways dashboard + Create NAT Gateway](/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/1-open-nat-gateways-dashboard-create-nat-gateway.png)

### Step 2 - Configure NAT Gateway settings

**Values used in this step**

**NAT Gateway value**

```text
nexusoms-demo-nat-a | nexusoms-demo-public-a | nexusoms-demo-nat-eip-a
```

**NAT Gateway value**

```text
nexusoms-demo-nat-b | nexusoms-demo-public-b | nexusoms-demo-nat-eip-b
```

![Configure NAT Gateway settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/2-configure-nat-gateway-settings.png)

### Step 3 - Click Create NAT Gateway

![Click Create NAT Gateway](/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/3-click-create-nat-gateway.png)
