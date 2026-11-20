---
title: "CloudWatch Alarm"
date: 2026-11-20
weight: 2
chapter: false
---

Tạo hai alarm sau khi ALB, target group và SQS đã hoạt động.

## 1. Lấy dimension của ALB và target group

Mở PowerShell:

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

## 2. Tạo alarm cho unhealthy target

```powershell
aws cloudwatch put-metric-alarm --profile $AwsProfile --region $AwsRegion --alarm-name nexusoms-demo-alb-unhealthy-targets --namespace AWS/ApplicationELB --metric-name UnHealthyHostCount --dimensions "Name=LoadBalancer,Value=$AlbDimension" "Name=TargetGroup,Value=$TargetGroupDimension" --statistic Minimum --period 60 --evaluation-periods 3 --datapoints-to-alarm 2 --threshold 0 --comparison-operator GreaterThanThreshold --treat-missing-data notBreaching
```

Alarm chuyển sang `ALARM` khi target unhealthy trong ít nhất hai trên ba khoảng một phút.

## 3. Tạo alarm cho dead-letter queue

```powershell
aws cloudwatch put-metric-alarm --profile $AwsProfile --region $AwsRegion --alarm-name nexusoms-demo-webhook-dlq-visible --namespace AWS/SQS --metric-name ApproximateNumberOfMessagesVisible --dimensions "Name=QueueName,Value=nexusoms-demo-webhook-dlq.fifo" --statistic Maximum --period 60 --evaluation-periods 3 --datapoints-to-alarm 2 --threshold 0 --comparison-operator GreaterThanThreshold --treat-missing-data notBreaching
```

Alarm chuyển sang `ALARM` khi DLQ có message trong ít nhất hai trên ba khoảng một phút.

## 4. Kiểm tra

```powershell
aws cloudwatch describe-alarms --profile $AwsProfile --region $AwsRegion --alarm-names nexusoms-demo-alb-unhealthy-targets nexusoms-demo-webhook-dlq-visible --query "MetricAlarms[].{Name:AlarmName,State:StateValue,Metric:MetricName}" --output table
```

Mở **CloudWatch → Alarms → All alarms** và xác nhận có hai alarm. Bài lab không cấu hình SNS nên alarm chỉ hiển thị trạng thái trong CloudWatch.
