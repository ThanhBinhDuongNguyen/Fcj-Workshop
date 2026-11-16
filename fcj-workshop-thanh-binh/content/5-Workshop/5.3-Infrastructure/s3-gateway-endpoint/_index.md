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

Complete **Open Endpoints dashboard + Create endpoint**.

**Values used in this step**

**S3 gateway endpoint - input 1**

```text
com.amazonaws.ap-southeast-1.s3
```

![Open Endpoints dashboard + Create endpoint](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/1-open-endpoints-dashboard-create-endpoint.png)

### Step 2 - Fill name tag + Choose type

Complete **Fill name tag + Choose type**.

![Fill name tag + Choose type](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/2-fill-name-tag-choose-type.png)

### Step 3 - Choose S3 service

Complete **Choose S3 service**.

![Choose S3 service](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/3-choose-s3-service.png)

### Step 4 - Choose 2 private route tables + Full access for policy

Complete **Choose 2 private route tables + Full access for policy**.

**Values used in this step**

**Private subnet value**

```text
nexusoms-demo-private-a-rt
```

**Private subnet value**

```text
nexusoms-demo-private-b-rt
```

![Choose 2 private route tables + Full access for policy](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/4-choose-2-private-route-tables-full-access-for-policy.png)

### Step 5 - Click create endpoint

Complete **Click create endpoint**.

![Click create endpoint](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/5-click-create-endpoint.png)
