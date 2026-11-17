---
title: "Dọn sạch tài nguyên AWS"
date: 2026-11-15
weight: 1
chapter: false
---

## 1. CloudFront và frontend bucket

1. Disable CloudFront distribution.

![Disable CloudFront distribution](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/1-disable-distribution.png)

2. Chờ distribution cập nhật xong rồi xóa distribution.

![Xóa CloudFront distribution](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/2-delete-distribution.png)

3. Xóa CloudFront Function.

![Xóa CloudFront Function](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/3-delete-cloudfront-functions.png)

4. Empty và xóa frontend bucket.

![Empty frontend bucket](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/19-empty-s3-bucket.png)

![Xóa frontend bucket](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/20-delete-s3-bucket.png)

## 2. ECS, ALB và ECR

1. ECS service → Update desired count về `0` → Delete service.
2. Xóa ECS cluster.

![Xóa ECS cluster](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/5-delete-cluster.png)

3. Deregister task definition revisions.

![Chọn task definition](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/13-choose-nexusoms-demo-api-task.png)

![Deregister task definition](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/14-deregister-the-task-revision.png)

4. Mở ALB attributes và tắt access logs.

![Mở ALB attributes](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/22-open-alb-attributes.png)

![Tắt ALB access logs](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/23-disable-alb-access-logs.png)

5. Xóa ALB rồi xóa target group.

![Xóa ALB](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/7-delete-load-balancer.png)

![Xóa target group](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/6-delete-target-group.png)

6. Xóa ECR repository và images.

![Xóa ECR repository](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/15-delete-repositories.png)

## 3. SQS

1. Bảo đảm ECS service đã dừng.
2. Chọn `nexusoms-demo-webhook-queue.fifo` → **Delete**.

![Xóa SQS queue](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/30-delete-sqs-queue.png)

3. Nhập tên queue để xác nhận.

![Xác nhận xóa SQS queue](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/31-confirm-delete-sqs-queue.png)

4. Xóa `nexusoms-demo-webhook-dlq.fifo` theo cùng cách.

## 4. CloudTrail và audit bucket

1. CloudTrail → `nexusoms-demo-trail` → **Stop logging** → Delete trail.

![Xóa CloudTrail trail](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/32-delete-cloudtrail-trail.png)
2. Empty audit bucket.

![Empty audit bucket](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/24-empty-audit-bucket.png)

![Xác nhận empty bucket](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/25-confirm-empty-audit-bucket.png)

3. Xóa audit bucket.

![Xóa audit bucket](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/26-delete-audit-bucket.png)

## 5. Network

1. Xóa S3 Gateway Endpoint.
2. Xóa NAT Gateway B và A.

![Xóa NAT Gateway B](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/8-delete-nat-gateway-b.png)

![Xóa NAT Gateway A](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/9-delete-nat-gateway-a.png)

3. Chờ NAT Gateway chuyển sang `Deleted`, sau đó release hai Elastic IP.

![Release Elastic IP](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/10-release-2-elastic-ip-addresses.png)

4. Xóa route table, subnet và custom security group.
5. Detach rồi xóa Internet Gateway.

![Detach Internet Gateway](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/11-detach-igw-from-vpc.png)

6. Xóa VPC.

![Xóa VPC](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/12-delete-vpc.png)

## 6. IAM, secret và log

1. Xóa secret `nexusoms/demo/application`.

![Mở secret](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/16-open-detail-secrets-manager.png)

![Schedule secret deletion](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/17-select-waiting-period-schedule-deletion.png)

2. Xóa log group `/ecs/nexusoms-demo-api`.

![Xóa CloudWatch log group](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/18-delete-log-groups-in-cloudwatch.png)

3. Xóa GitHub Actions role.

![Xóa GitHub Actions role](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/27-delete-github-actions-role.png)

4. Xóa ECS execution role và task role.

![Xóa ECS roles](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/28-delete-ecs-roles.png)

5. Xóa GitHub OIDC provider nếu không còn repository nào sử dụng.

![Xóa GitHub OIDC provider](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/29-delete-github-oidc-provider.png)

6. Xóa IAM deployer user và access key.

## 7. Kiểm tra sau cleanup

Kiểm tra lại NAT Gateway, Elastic IP, ALB, Target Group, ECS, ECR, SQS, CloudFront, S3, CloudTrail, Secrets Manager và CloudWatch. Giữ hoặc xóa Supabase project theo mục tiêu bài lab.
