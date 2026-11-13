---
title: "Allocate Elastic IP addresses"
date: 2026-11-13
weight: 4
chapter: false
---

Allocate two Elastic IP addresses in `ap-southeast-1`. The screenshots show the first run; repeat it with the second Name tag. These addresses are attached to NAT gateways A and B.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**NAT Gateway value**

```text
nexusoms-demo-nat-eip-a
```

**NAT Gateway value**

```text
nexusoms-demo-nat-eip-b
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open Elastic IPs + Allocate Elastic IP

In AWS Console, complete **Open Elastic IPs + Allocate Elastic IP** as shown below.


{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/elastic-ips/1. Open Elastic IPs + Allocate Elastic IP.png" title="Open Elastic IPs + Allocate Elastic IP" >}}

### Step 2 - Fill the name tag for Elastic IP + Allocate

In AWS Console, complete **Fill the name tag for Elastic IP + Allocate** as shown below.

**Values used in this step**

**NAT Gateway value**

```text
nexusoms-demo-nat-eip-a
```

**NAT Gateway value**

```text
nexusoms-demo-nat-eip-b
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/elastic-ips/2. Fill the name tag for Elastic IP + Allocate.png" title="Fill the name tag for Elastic IP + Allocate" >}}
