---
title: "Create the VPC"
date: 2026-11-13
weight: 1
chapter: false
---

Create the private network boundary for the workshop and enable both DNS resolution and DNS hostnames.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**Region**

```text
ap-southeast-1
```

**VPC value**

```text
nexusoms-demo-vpc
```

**IPv4 CIDR**

```text
10.80.0.0/16
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open Your VPCs dashboard

Complete **Open Your VPCs dashboard**.


![Open Your VPCs dashboard](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/1-open-your-vpcs-dashboard.png)

### Step 2 - Click create VPC

Complete **Click create VPC**.


![Click create VPC](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/2-click-create-vpc.png)

### Step 3 - Configure VPC settings

Complete **Configure VPC settings**.

**Values used in this step**

**Region**

```text
ap-southeast-1
```

**VPC value**

```text
nexusoms-demo-vpc
```

**IPv4 CIDR**

```text
10.80.0.0/16
```

![Configure VPC settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/3-configure-vpc-settings.png)

### Step 4 - Choose Action + Edit VPC Settings

Complete **Choose Action + Edit VPC Settings**.

![Choose Action + Edit VPC Settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/4-choose-action-edit-vpc-settings.png)

### Step 5 - Check 2 box in DNS settings

Complete **Check 2 box in DNS settings**.

![Check 2 box in DNS settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/5-check-2-box-in-dns-settings.png)

### Step 6 - Copy VPC ID

Complete **Copy VPC ID**.

![Copy VPC ID](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/6-copy-vpc-id.png)
