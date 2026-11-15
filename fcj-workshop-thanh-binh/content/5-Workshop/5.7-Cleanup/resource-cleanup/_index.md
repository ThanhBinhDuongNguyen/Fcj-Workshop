---
title: "Clean up AWS resources"
date: 2026-11-15
weight: 1
chapter: false
---

## 1. CloudFront and the frontend bucket

1. Disable the CloudFront distribution.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/1-disable-distribution.png" title="Disable the CloudFront distribution" >}}

2. Wait for deployment, then delete the distribution.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/2-delete-distribution.png" title="Delete the CloudFront distribution" >}}

3. Delete the CloudFront Function.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/3-delete-cloudfront-functions.png" title="Delete the CloudFront Function" >}}

4. Empty and delete the frontend bucket.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/19-empty-s3-bucket.png" title="Empty the frontend bucket" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/20-delete-s3-bucket.png" title="Delete the frontend bucket" >}}

## 2. ECS, ALB, and ECR

1. Set the ECS service desired count to `0`, then delete the service.
2. Delete the ECS cluster.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/5-delete-cluster.png" title="Delete the ECS cluster" >}}

3. Deregister task definition revisions.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/13-choose-nexusoms-demo-api-task.png" title="Select the task definition" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/14-deregister-the-task-revision.png" title="Deregister the task definition" >}}

4. Open ALB attributes and disable access logs.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/22-open-alb-attributes.png" title="Open ALB attributes" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/23-disable-alb-access-logs.png" title="Disable ALB access logs" >}}

5. Delete the ALB, then delete the target group.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/7-delete-load-balancer.png" title="Delete the ALB" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/6-delete-target-group.png" title="Delete the target group" >}}

6. Delete the ECR repository and its images.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/15-delete-repositories.png" title="Delete the ECR repository" >}}

## 3. CloudTrail and the audit bucket

1. Stop logging and delete `nexusoms-demo-trail`.
2. Empty the audit bucket.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/24-empty-audit-bucket.png" title="Empty the audit bucket" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/25-confirm-empty-audit-bucket.png" title="Confirm the empty bucket" >}}

3. Delete the audit bucket.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/26-delete-audit-bucket.png" title="Delete the audit bucket" >}}

## 4. Network

1. Delete the S3 gateway endpoint.
2. Delete NAT gateways B and A.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/8-delete-nat-gateway-b.png" title="Delete NAT gateway B" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/9-delete-nat-gateway-a.png" title="Delete NAT gateway A" >}}

3. Wait until both NAT gateways are deleted, then release both Elastic IP addresses.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/10-release-2-elastic-ip-addresses.png" title="Release the Elastic IP addresses" >}}

4. Delete route tables, subnets, and custom security groups.
5. Detach and delete the Internet Gateway.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/11-detach-igw-from-vpc.png" title="Detach the Internet Gateway" >}}

6. Delete the VPC.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/12-delete-vpc.png" title="Delete the VPC" >}}

## 5. IAM, secret, and logs

1. Schedule deletion of `nexusoms/demo/application`.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/16-open-detail-secrets-manager.png" title="Open the secret" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/17-select-waiting-period-schedule-deletion.png" title="Schedule secret deletion" >}}

2. Delete `/ecs/nexusoms-demo-api`.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/18-delete-log-groups-in-cloudwatch.png" title="Delete the CloudWatch log group" >}}

3. Delete the GitHub Actions role.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/27-delete-github-actions-role.png" title="Delete the GitHub Actions role" >}}

4. Delete the ECS execution role and task role.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/28-delete-ecs-roles.png" title="Delete the ECS roles" >}}

5. Delete the GitHub OIDC provider if no other repository uses it.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/29-delete-github-oidc-provider.png" title="Delete the GitHub OIDC provider" >}}

6. Delete the lab IAM user and access key.

## 6. Verify cleanup

Check NAT gateways, Elastic IP addresses, ALB, target groups, ECS, ECR, CloudFront, S3, CloudTrail, Secrets Manager, and CloudWatch. Keep or delete the Supabase project according to the lab goal.
