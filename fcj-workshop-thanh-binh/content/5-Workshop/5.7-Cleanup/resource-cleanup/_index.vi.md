---
title: "Dọn sạch tài nguyên AWS"
date: 2026-11-15
weight: 1
chapter: false
---

## 1. CloudFront và frontend bucket

1. Disable CloudFront distribution.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/1-disable-distribution.png" title="Disable CloudFront distribution" >}}

2. Chờ distribution cập nhật xong rồi xóa distribution.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/2-delete-distribution.png" title="Xóa CloudFront distribution" >}}

3. Xóa CloudFront Function.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/3-delete-cloudfront-functions.png" title="Xóa CloudFront Function" >}}

4. Empty và xóa frontend bucket.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/19-empty-s3-bucket.png" title="Empty frontend bucket" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/20-delete-s3-bucket.png" title="Xóa frontend bucket" >}}

## 2. ECS, ALB và ECR

1. ECS service → Update desired count về `0` → Delete service.
2. Xóa ECS cluster.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/5-delete-cluster.png" title="Xóa ECS cluster" >}}

3. Deregister task definition revisions.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/13-choose-nexusoms-demo-api-task.png" title="Chọn task definition" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/14-deregister-the-task-revision.png" title="Deregister task definition" >}}

4. Mở ALB attributes và tắt access logs.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/22-open-alb-attributes.png" title="Mở ALB attributes" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/23-disable-alb-access-logs.png" title="Tắt ALB access logs" >}}

5. Xóa ALB rồi xóa target group.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/7-delete-load-balancer.png" title="Xóa ALB" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/6-delete-target-group.png" title="Xóa target group" >}}

6. Xóa ECR repository và images.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/15-delete-repositories.png" title="Xóa ECR repository" >}}

## 3. CloudTrail và audit bucket

1. CloudTrail → `nexusoms-demo-trail` → **Stop logging** → Delete trail.
2. Empty audit bucket.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/24-empty-audit-bucket.png" title="Empty audit bucket" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/25-confirm-empty-audit-bucket.png" title="Xác nhận empty bucket" >}}

3. Xóa audit bucket.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/26-delete-audit-bucket.png" title="Xóa audit bucket" >}}

## 4. Network

1. Xóa S3 Gateway Endpoint.
2. Xóa NAT Gateway B và A.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/8-delete-nat-gateway-b.png" title="Xóa NAT Gateway B" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/9-delete-nat-gateway-a.png" title="Xóa NAT Gateway A" >}}

3. Chờ NAT Gateway chuyển sang `Deleted`, sau đó release hai Elastic IP.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/10-release-2-elastic-ip-addresses.png" title="Release Elastic IP" >}}

4. Xóa route table, subnet và custom security group.
5. Detach rồi xóa Internet Gateway.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/11-detach-igw-from-vpc.png" title="Detach Internet Gateway" >}}

6. Xóa VPC.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/12-delete-vpc.png" title="Xóa VPC" >}}

## 5. IAM, secret và log

1. Xóa secret `nexusoms/demo/application`.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/16-open-detail-secrets-manager.png" title="Mở secret" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/17-select-waiting-period-schedule-deletion.png" title="Schedule secret deletion" >}}

2. Xóa log group `/ecs/nexusoms-demo-api`.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/18-delete-log-groups-in-cloudwatch.png" title="Xóa CloudWatch log group" >}}

3. Xóa GitHub Actions role.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/27-delete-github-actions-role.png" title="Xóa GitHub Actions role" >}}

4. Xóa ECS execution role và task role.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/28-delete-ecs-roles.png" title="Xóa ECS roles" >}}

5. Xóa GitHub OIDC provider nếu không còn repository nào sử dụng.

{{< figure src="/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/29-delete-github-oidc-provider.png" title="Xóa GitHub OIDC provider" >}}

6. Xóa IAM deployer user và access key.

## 6. Kiểm tra sau cleanup

Kiểm tra lại NAT Gateway, Elastic IP, ALB, Target Group, ECS, ECR, CloudFront, S3, CloudTrail, Secrets Manager và CloudWatch. Giữ hoặc xóa Supabase project theo mục tiêu bài lab.
