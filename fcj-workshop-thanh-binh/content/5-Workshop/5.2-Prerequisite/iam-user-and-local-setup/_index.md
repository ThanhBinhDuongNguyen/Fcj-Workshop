---
title: "Sign in to AWS CLI"
date: 2026-11-20
weight: 5
chapter: false
---

Sign in to AWS CLI with the `nexusoms-demo` profile. Do not create a long-lived access key for this lab.

## 1. Sign in

Open PowerShell:

![Open PowerShell](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/13-open-powershell.png)

```powershell
aws --version
aws login --profile nexusoms-demo --region ap-southeast-1
```

Complete the browser sign-in opened by AWS CLI.

## 2. Verify the account

```powershell
$AwsProfile = "nexusoms-demo"
$AwsRegion = "ap-southeast-1"

aws sts get-caller-identity `
  --profile $AwsProfile `
  --region $AwsRegion
```

Match `Account` with the AWS Account ID shown in the Console.

## 3. Check Availability Zones

```powershell
aws ec2 describe-availability-zones `
  --profile $AwsProfile `
  --region $AwsRegion `
  --query "AvailabilityZones[].ZoneName" `
  --output table
```

## 4. Get the CloudFront managed prefix list ID

```powershell
$CloudFrontPrefixListId = aws ec2 describe-managed-prefix-lists --profile $AwsProfile --region $AwsRegion --filters "Name=prefix-list-name,Values=com.amazonaws.global.cloudfront.origin-facing" --query "PrefixLists[0].PrefixListId" --output text
$CloudFrontPrefixListId
```

![Verify the account and CloudFront prefix list](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/16-get-available-zone-cloudfront.png)

Expected result:

- STS returns the intended AWS account.
- The selected Region is `ap-southeast-1`.
- The prefix list ID starts with `pl-`.
