---
title: "Clean up AWS resources"
date: 2026-11-15
weight: 1
chapter: false
---

## 1. CloudFront and the frontend bucket

1. Disable the CloudFront distribution.

![Disable the CloudFront distribution](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/1-disable-distribution.png)

2. Wait for deployment, then delete the distribution.

![Delete the CloudFront distribution](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/2-delete-distribution.png)

3. Delete the CloudFront Function.

![Delete the CloudFront Function](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/3-delete-cloudfront-functions.png)

4. Empty and delete the frontend bucket.

![Empty the frontend bucket](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/19-empty-s3-bucket.png)

![Delete the frontend bucket](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/20-delete-s3-bucket.png)

## 2. ECS, ALB, and ECR

1. Set the ECS service desired count to `0`, then delete the service.
2. Delete the ECS cluster.

![Delete the ECS cluster](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/5-delete-cluster.png)

3. Deregister task definition revisions.

![Select the task definition](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/13-choose-nexusoms-demo-api-task.png)

![Deregister the task definition](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/14-deregister-the-task-revision.png)

4. Open ALB attributes and disable access logs.

![Open ALB attributes](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/22-open-alb-attributes.png)

![Disable ALB access logs](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/23-disable-alb-access-logs.png)

5. Delete the ALB, then delete the target group.

![Delete the ALB](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/7-delete-load-balancer.png)

![Delete the target group](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/6-delete-target-group.png)

6. Delete the ECR repository and its images.

![Delete the ECR repository](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/15-delete-repositories.png)

## 3. SQS

1. Confirm that the ECS service has stopped.
2. Select `nexusoms-demo-webhook-queue.fifo` and choose **Delete**.

![Delete the SQS queue](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/30-delete-sqs-queue.png)

3. Enter the queue name to confirm.

![Confirm SQS queue deletion](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/31-confirm-delete-sqs-queue.png)

4. Delete `nexusoms-demo-webhook-dlq.fifo` the same way.

## 4. CloudTrail and the audit bucket

1. Stop logging and delete `nexusoms-demo-trail`.

![Delete the CloudTrail trail](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/32-delete-cloudtrail-trail.png)
2. Empty the audit bucket.

![Empty the audit bucket](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/24-empty-audit-bucket.png)

![Confirm the empty bucket](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/25-confirm-empty-audit-bucket.png)

3. Delete the audit bucket.

![Delete the audit bucket](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/26-delete-audit-bucket.png)

## 5. Network

1. Delete the S3 gateway endpoint.
2. Delete NAT gateways B and A.

![Delete NAT gateway B](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/8-delete-nat-gateway-b.png)

![Delete NAT gateway A](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/9-delete-nat-gateway-a.png)

3. Wait until both NAT gateways are deleted, then release both Elastic IP addresses.

![Release the Elastic IP addresses](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/10-release-2-elastic-ip-addresses.png)

4. Delete route tables, subnets, and custom security groups.
5. Detach and delete the Internet Gateway.

![Detach the Internet Gateway](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/11-detach-igw-from-vpc.png)

6. Delete the VPC.

![Delete the VPC](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/12-delete-vpc.png)

## 6. IAM, secret, and logs

1. Schedule deletion of `nexusoms/demo/application`.

![Open the secret](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/16-open-detail-secrets-manager.png)

![Schedule secret deletion](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/17-select-waiting-period-schedule-deletion.png)

2. Delete both CloudWatch alarms:

```powershell
aws cloudwatch delete-alarms --profile nexusoms-demo --region ap-southeast-1 --alarm-names nexusoms-demo-alb-unhealthy-targets nexusoms-demo-webhook-dlq-visible
```

3. Delete `/ecs/nexusoms-demo-api`.

![Delete the CloudWatch log group](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/18-delete-log-groups-in-cloudwatch.png)

4. Delete the GitHub Actions role.

![Delete the GitHub Actions role](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/27-delete-github-actions-role.png)

5. Delete the ECS execution role and task role.

![Delete the ECS roles](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/28-delete-ecs-roles.png)

6. Delete the GitHub OIDC provider if no other repository uses it.

![Delete the GitHub OIDC provider](/images/5-Workshop/nexusoms/5.7-Cleanup/resource-cleanup/29-delete-github-oidc-provider.png)

## 7. Verify cleanup

Check NAT gateways, Elastic IP addresses, ALB, target groups, ECS, ECR, SQS, CloudFront, S3, CloudTrail, Secrets Manager, and CloudWatch. Keep or delete the Supabase project according to the lab goal.
