---
title: "Create NAT gateways"
date: 2026-11-13
weight: 5
chapter: false
---

Each Availability Zone receives its own NAT gateway so private ECS tasks can reach Supabase and external webhook endpoints.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

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

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open NAT Gateways dashboard + Create NAT Gateway

In AWS Console, complete **Open NAT Gateways dashboard + Create NAT Gateway** as shown below.


{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/1. Open NAT Gateways dashboard + Create NAT Gateway.png" title="Open NAT Gateways dashboard + Create NAT Gateway" >}}

### Step 2 - Configure NAT Gateway settings

In AWS Console, complete **Configure NAT Gateway settings** as shown below.

**Values used in this step**

**NAT Gateway value**

```text
nexusoms-demo-nat-a | nexusoms-demo-public-a | nexusoms-demo-nat-eip-a
```

**NAT Gateway value**

```text
nexusoms-demo-nat-b | nexusoms-demo-public-b | nexusoms-demo-nat-eip-b
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/2. Configure NAT Gateway settings.png" title="Configure NAT Gateway settings" >}}

### Step 3 - Click Create NAT Gateway

In AWS Console, complete **Click Create NAT Gateway** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/3. Click Create NAT Gateway.png" title="Click Create NAT Gateway" >}}
