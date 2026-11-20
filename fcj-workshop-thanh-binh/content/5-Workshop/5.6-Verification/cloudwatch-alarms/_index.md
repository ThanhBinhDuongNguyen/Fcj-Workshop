---
title: "CloudWatch alarms"
date: 2026-11-20
weight: 2
chapter: false
---

Create these alarms after the ALB, target group, and SQS queues are running.

## 1. Get the ALB and target-group dimensions

Open PowerShell:

```powershell
$AwsProfile = "nexusoms-demo"
$AwsRegion = "ap-southeast-1"
$AlbArn = aws elbv2 describe-load-balancers --profile $AwsProfile --region $AwsRegion --names nexusoms-demo-alb --query "LoadBalancers[0].LoadBalancerArn" --output text
$TargetGroupArn = aws elbv2 describe-target-groups --profile $AwsProfile --region $AwsRegion --names nexusoms-demo-api-tg --query "TargetGroups[0].TargetGroupArn" --output text
$AlbDimension = $AlbArn.Substring($AlbArn.IndexOf("loadbalancer/") + "loadbalancer/".Length)
$TargetGroupDimension = $TargetGroupArn.Substring($TargetGroupArn.IndexOf("targetgroup/"))
"ALB_DIMENSION=$AlbDimension"
"TARGET_GROUP_DIMENSION=$TargetGroupDimension"
```

## 2. Create the unhealthy-target alarm

```powershell
aws cloudwatch put-metric-alarm --profile $AwsProfile --region $AwsRegion --alarm-name nexusoms-demo-alb-unhealthy-targets --namespace AWS/ApplicationELB --metric-name UnHealthyHostCount --dimensions "Name=LoadBalancer,Value=$AlbDimension" "Name=TargetGroup,Value=$TargetGroupDimension" --statistic Minimum --period 60 --evaluation-periods 3 --datapoints-to-alarm 2 --threshold 0 --comparison-operator GreaterThanThreshold --treat-missing-data notBreaching
```

The alarm enters `ALARM` when the target is unhealthy for at least two of three one-minute periods.

## 3. Create the dead-letter queue alarm

```powershell
aws cloudwatch put-metric-alarm --profile $AwsProfile --region $AwsRegion --alarm-name nexusoms-demo-webhook-dlq-visible --namespace AWS/SQS --metric-name ApproximateNumberOfMessagesVisible --dimensions "Name=QueueName,Value=nexusoms-demo-webhook-dlq.fifo" --statistic Maximum --period 60 --evaluation-periods 3 --datapoints-to-alarm 2 --threshold 0 --comparison-operator GreaterThanThreshold --treat-missing-data notBreaching
```

The alarm enters `ALARM` when the DLQ contains messages for at least two of three one-minute periods.

## 4. Verify

```powershell
aws cloudwatch describe-alarms --profile $AwsProfile --region $AwsRegion --alarm-names nexusoms-demo-alb-unhealthy-targets nexusoms-demo-webhook-dlq-visible --query "MetricAlarms[].{Name:AlarmName,State:StateValue,Metric:MetricName}" --output table
```

Open **CloudWatch → Alarms → All alarms** and confirm that both alarms exist. The lab does not configure SNS, so alarm state is visible only in CloudWatch.
