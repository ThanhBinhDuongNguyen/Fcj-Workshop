---
title: "Target group"
date: 2026-11-13
weight: 3
chapter: false
---

Create an **IP addresses** target group because Fargate uses `awsvpc`. Select `nexusoms-demo-vpc`, port `8080`, and `/health`; do not register IP addresses manually.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**Target group - input 1**

```text
nexusoms-demo-api-tg
```

**Target type**

```text
Target type: IP addresses
```

**Protocol**

```text
Protocol: HTTP
```

**Port**

```text
Port: 8080
```

**Health check path**

```text
Health check path: /health
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open Target Groups dashboard + Create target group

Complete **Open Target Groups dashboard + Create target group**.


![Open Target Groups dashboard + Create target group](/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/1-open-target-groups-dashboard-create-target-group.png)

### Step 2 - Configure target group

Complete **Configure target group**.

**Values used in this step**

**Target group - input 1**

```text
nexusoms-demo-api-tg
```

**Target type**

```text
Target type: IP addresses
```

**Protocol**

```text
Protocol: HTTP
```

**Port**

```text
Port: 8080
```

![Configure target group](/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/2-configure-target-group.png)

### Step 3 - Choose VPC + Set up health checks

Complete **Choose VPC + Set up health checks**.

**Values used in this step**

**Health check path**

```text
Health check path: /health
```

![Choose VPC + Set up health checks](/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/3-choose-vpc-set-up-health-checks.png)

### Step 4 - Complete create target group

Complete **Complete create target group**.

![Complete create target group](/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/4-complete-create-target-group.png)
