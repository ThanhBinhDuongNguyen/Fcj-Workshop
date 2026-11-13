---
title: "Application Load Balancer"
date: 2026-11-13
weight: 5
chapter: false
---

The ALB spans both public subnets and forwards CloudFront requests to the ECS target group.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**Application Load Balancer value**

```text
nexusoms-demo-alb
```

**Application Load Balancer - input 2**

```text
Internet-facing
```

**Application Load Balancer - input 3**

```text
IPv4
```

**Application Load Balancer - input 4**

```text
HTTP:80
```

**Application Load Balancer - input 5**

```text
nexusoms-demo-api-tg
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open Load Balancers dashboard + Create load balancer

In AWS Console, complete **Open Load Balancers dashboard + Create load balancer** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/1. Open Load Balancers dashboard + Create load balancer.png" title="Open Load Balancers dashboard + Create load balancer" >}}

### Step 2 - Choose create Application Load Balancer

In AWS Console, complete **Choose create Application Load Balancer** as shown below.

**Values used in this step**

**Application Load Balancer value**

```text
nexusoms-demo-alb
```

**Application Load Balancer - input 2**

```text
Internet-facing
```

**Application Load Balancer - input 3**

```text
IPv4
```

**Application Load Balancer - input 4**

```text
HTTP:80
```

**Application Load Balancer - input 5**

```text
nexusoms-demo-api-tg
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/2. Choose create Application Load Balancer.png" title="Choose create Application Load Balancer" >}}

### Step 3 - Configure basic Load Balancer

In AWS Console, complete **Configure basic Load Balancer** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/3. Configure basic Load Balancer.png" title="Configure basic Load Balancer" >}}

### Step 4 - Configure Network mapping for Load Balancer

In AWS Console, complete **Configure Network mapping for Load Balancer** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/4. Configure Network mapping for Load Balancer.png" title="Configure Network mapping for Load Balancer" >}}

### Step 5 - Configure Security group for Load Balancer

In AWS Console, complete **Configure Security group for Load Balancer** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/5. Configure Security group for Load Balancer.png" title="Configure Security group for Load Balancer" >}}

### Step 6 - Configure Target group for Load Balancer

In AWS Console, complete **Configure Target group for Load Balancer** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/6. Configure Target group for Load Balancer.png" title="Configure Target group for Load Balancer" >}}

### Step 7 - Copy Load balancer ARN

In AWS Console, complete **Copy Load balancer ARN** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/7. Copy Load balancer ARN.png" title="Copy Load balancer ARN" >}}
