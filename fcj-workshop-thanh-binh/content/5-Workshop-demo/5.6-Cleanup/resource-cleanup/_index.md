---
title: "Delete all workshop resources"
date: 2026-11-13
weight: 1
chapter: false
---

Follow dependency order. Confirm that NAT gateways, Elastic IPs, the ALB, ECS service, and endpoints are deleted because these resources can continue accruing charges.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**ECS service name**

```text
nexusoms-demo-api-service
```

**Application Load Balancer value**

```text
nexusoms-demo-alb
```

**Delete all workshop resources - input 3**

```text
nexusoms-demo-api-tg
```

**ECS cluster name**

```text
nexusoms-demo-cluster
```

**Delete all workshop resources - input 5**

```text
nexusoms-demo-api
```

**Delete all workshop resources - input 6**

```text
nexusoms/demo/application
```

**Delete all workshop resources - input 7**

```text
/ecs/nexusoms-demo-api
```

**Frontend resource name**

```text
nexusoms-demo-frontend-<AWS_ACCOUNT_ID>
```

**VPC value**

```text
nexusoms-demo-vpc
```

**Delete all workshop resources - input 10**

```text
nexusoms-demo-deployer
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Disable Distribution

In AWS Console, complete **Disable Distribution** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/1. Disable Distribution.png" title="Disable Distribution" >}}

### Step 2 - Delete Distribution

In AWS Console, complete **Delete Distribution** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/2. Delete Distribution.png" title="Delete Distribution" >}}

### Step 3 - Delete CloudFront functions

In AWS Console, complete **Delete CloudFront functions** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/3. Delete CloudFront functions.png" title="Delete CloudFront functions" >}}

### Step 4 - Delete Stacks in CloudFormation

In AWS Console, complete **Delete Stacks in CloudFormation** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/4. Delete Stacks in CloudFormation.png" title="Delete Stacks in CloudFormation" >}}

### Step 5 - Delete cluster

In AWS Console, complete **Delete cluster** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/5. Delete cluster.png" title="Delete cluster" >}}

### Step 6 - Delete Target Group

In AWS Console, complete **Delete Target Group** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/6. Delete Target Group.png" title="Delete Target Group" >}}

### Step 7 - Delete Load Balancer

In AWS Console, complete **Delete Load Balancer** as shown below.

**Values used in this step**

**Application Load Balancer value**

```text
nexusoms-demo-alb
```

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/7. Delete Load Balancer.png" title="Delete Load Balancer" >}}

### Step 8 - Delete NAT Gateway B

In AWS Console, complete **Delete NAT Gateway B** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/8. Delete NAT Gateway B.png" title="Delete NAT Gateway B" >}}

### Step 9 - Delete NAT Gateway A

In AWS Console, complete **Delete NAT Gateway A** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/9. Delete NAT Gateway A.png" title="Delete NAT Gateway A" >}}

### Step 10 - Release 2 Elastic IP addresses

In AWS Console, complete **Release 2 Elastic IP addresses** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/10. Release 2 Elastic IP addresses.png" title="Release 2 Elastic IP addresses" >}}

### Step 11 - Detach IGW from VPC

In AWS Console, complete **Detach IGW from VPC** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/11. Detach IGW from VPC.png" title="Detach IGW from VPC" >}}

### Step 12 - Delete VPC

In AWS Console, complete **Delete VPC** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/12. Delete VPC.png" title="Delete VPC" >}}

### Step 13 - Choose Nexusoms-demo-api task

In AWS Console, complete **Choose Nexusoms-demo-api task** as shown below.

**Values used in this step**

**ECS service name**

```text
nexusoms-demo-api-service
```

**Delete all workshop resources - input 3**

```text
nexusoms-demo-api-tg
```

**ECS cluster name**

```text
nexusoms-demo-cluster
```

**Delete all workshop resources - input 5**

```text
nexusoms-demo-api
```

**Delete all workshop resources - input 6**

```text
nexusoms/demo/application
```

**Delete all workshop resources - input 7**

```text
/ecs/nexusoms-demo-api
```

**Frontend resource name**

```text
nexusoms-demo-frontend-<AWS_ACCOUNT_ID>
```

**VPC value**

```text
nexusoms-demo-vpc
```

**Delete all workshop resources - input 10**

```text
nexusoms-demo-deployer
```

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/13. Choose Nexusoms-demo-api task.png" title="Choose Nexusoms-demo-api task" >}}

### Step 14 - Deregister the Task revision

In AWS Console, complete **Deregister the Task revision** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/14. Deregister the Task revision.png" title="Deregister the Task revision" >}}

### Step 15 - Delete Repositories

In AWS Console, complete **Delete Repositories** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/15. Delete Repositories.png" title="Delete Repositories" >}}

### Step 16 - Open detail Secrets Manager

In AWS Console, complete **Open detail Secrets Manager** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/16. Open detail Secrets Manager.png" title="Open detail Secrets Manager" >}}

### Step 17 - Select waiting period + schedule deletion

In AWS Console, complete **Select waiting period + schedule deletion** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/17. Select waiting period + schedule deletion.png" title="Select waiting period + schedule deletion" >}}

### Step 18 - Delete log groups in CloudWatch

In AWS Console, complete **Delete log groups in CloudWatch** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/18. Delete log groups in CloudWatch.png" title="Delete log groups in CloudWatch" >}}

### Step 19 - Empty S3 bucket

In AWS Console, complete **Empty S3 bucket** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/19. Empty S3 bucket.png" title="Empty S3 bucket" >}}

### Step 20 - Delete S3 bucket

In AWS Console, complete **Delete S3 bucket** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/20. Delete S3 bucket.png" title="Delete S3 bucket" >}}

### Step 21 - Delete 2 role that create

In AWS Console, complete **Delete 2 role that create** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Cleanup/resource-cleanup/21. Delete 2 role that create.png" title="Delete 2 role that create" >}}
