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

Complete **Open subnets dashboard + Create subnet**.

![Open subnets dashboard + Create subnet](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/1-open-subnets-dashboard-create-subnet.png)

### Step 2 - Choose VPC ID

Complete **Choose VPC ID**.

![Choose VPC ID](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/2-choose-vpc-id.png)

### Step 3 - Configure public subnet a

Complete **Configure public subnet a**.

**Values used in this step**

**Public subnet value**

```text
nexusoms-demo-public-a | 10.80.0.0/24
```

**Public subnet value**

```text
nexusoms-demo-public-b | 10.80.1.0/24
```

![Configure public subnet a](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/3-configure-public-subnet-a.png)

### Step 4 - Configure public subnet b

Complete **Configure public subnet b**.

![Configure public subnet b](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/4-configure-public-subnet-b.png)

### Step 5 - Configure private subnet a

Complete **Configure private subnet a**.

**Values used in this step**

**Private subnet value**

```text
nexusoms-demo-private-a | 10.80.10.0/24
```

**Private subnet value**

```text
nexusoms-demo-private-b | 10.80.11.0/24
```

![Configure private subnet a](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/5-configure-private-subnet-a.png)

### Step 6 - Configure private subnet b

Complete **Configure private subnet b**.

![Configure private subnet b](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/6-configure-private-subnet-b.png)

### Step 7 - Edit subnet settings

Complete **Edit subnet settings**.

![Edit subnet settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/7-edit-subnet-settings.png)

### Step 8 - Enable auto-assign public IPv4 address

Complete **Enable auto-assign public IPv4 address**.

![Enable auto-assign public IPv4 address](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/8-enable-auto-assign-public-ipv4-address.png)
