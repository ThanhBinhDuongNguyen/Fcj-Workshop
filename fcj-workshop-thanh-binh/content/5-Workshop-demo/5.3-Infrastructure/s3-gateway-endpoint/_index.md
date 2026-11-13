---
title: "S3 gateway endpoint"
date: 2026-11-13
weight: 8
chapter: false
---

The S3 gateway endpoint has no hourly charge and keeps private-subnet S3 traffic off the NAT gateways.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**S3 gateway endpoint - input 1**

```text
com.amazonaws.ap-southeast-1.s3
```

**Private subnet value**

```text
nexusoms-demo-private-a-rt
```

**Private subnet value**

```text
nexusoms-demo-private-b-rt
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open Endpoints dashboard + Create endpoint

In AWS Console, complete **Open Endpoints dashboard + Create endpoint** as shown below.

**Values used in this step**

**S3 gateway endpoint - input 1**

```text
com.amazonaws.ap-southeast-1.s3
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/1. Open Endpoints dashboard + Create endpoint.png" title="Open Endpoints dashboard + Create endpoint" >}}

### Step 2 - Fill name tag + Choose type

In AWS Console, complete **Fill name tag + Choose type** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/2. Fill name tag + Choose type.png" title="Fill name tag + Choose type" >}}

### Step 3 - Choose S3 service

In AWS Console, complete **Choose S3 service** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/3. Choose S3 service.png" title="Choose S3 service" >}}

### Step 4 - Choose 2 private route tables + Full access for policy

In AWS Console, complete **Choose 2 private route tables + Full access for policy** as shown below.

**Values used in this step**

**Private subnet value**

```text
nexusoms-demo-private-a-rt
```

**Private subnet value**

```text
nexusoms-demo-private-b-rt
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/4. Choose 2 private route tables + Full access for policy.png" title="Choose 2 private route tables + Full access for policy" >}}

### Step 5 - Click create endpoint

In AWS Console, complete **Click create endpoint** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/5. Click create endpoint.png" title="Click create endpoint" >}}
