---
title: "Create public and private subnets"
date: 2026-11-13
weight: 2
chapter: false
---

Create four subnets across two Availability Zones. Enable automatic public IPv4 assignment only on the two public subnets.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**Public subnet value**

```text
nexusoms-demo-public-a | 10.80.0.0/24
```

**Public subnet value**

```text
nexusoms-demo-public-b | 10.80.1.0/24
```

**Private subnet value**

```text
nexusoms-demo-private-a | 10.80.10.0/24
```

**Private subnet value**

```text
nexusoms-demo-private-b | 10.80.11.0/24
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open subnets dashboard + Create subnet

In AWS Console, complete **Open subnets dashboard + Create subnet** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/1. Open subnets dashboard + Create subnet.png" title="Open subnets dashboard + Create subnet" >}}

### Step 2 - Choose VPC ID

In AWS Console, complete **Choose VPC ID** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/2. Choose VPC ID.png" title="Choose VPC ID" >}}

### Step 3 - Configure public subnet a

In AWS Console, complete **Configure public subnet a** as shown below.

**Values used in this step**

**Public subnet value**

```text
nexusoms-demo-public-a | 10.80.0.0/24
```

**Public subnet value**

```text
nexusoms-demo-public-b | 10.80.1.0/24
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/3. Configure public subnet a.png" title="Configure public subnet a" >}}

### Step 4 - Configure public subnet b

In AWS Console, complete **Configure public subnet b** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/4. Configure public subnet b.png" title="Configure public subnet b" >}}

### Step 5 - Configure private subnet a

In AWS Console, complete **Configure private subnet a** as shown below.

**Values used in this step**

**Private subnet value**

```text
nexusoms-demo-private-a | 10.80.10.0/24
```

**Private subnet value**

```text
nexusoms-demo-private-b | 10.80.11.0/24
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/5. Configure private subnet a.png" title="Configure private subnet a" >}}

### Step 6 - Configure private subnet b

In AWS Console, complete **Configure private subnet b** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/6. Configure private subnet b.png" title="Configure private subnet b" >}}

### Step 7 - Edit subnet settings

In AWS Console, complete **Edit subnet settings** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/7. Edit subnet settings.png" title="Edit subnet settings" >}}

### Step 8 - Enable auto-assign public IPv4 address

In AWS Console, complete **Enable auto-assign public IPv4 address** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/8. Enable auto-assign public IPv4 address.png" title="Enable auto-assign public IPv4 address" >}}
