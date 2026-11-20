---
title: "Đăng nhập AWS CLI"
date: 2026-11-20
weight: 5
chapter: false
---

Đăng nhập AWS CLI bằng profile `nexusoms-demo`. Không tạo access key dài hạn cho bài lab.

## 1. Đăng nhập

Mở PowerShell:

![Mở PowerShell](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/13-open-powershell.png)

Chạy:

```powershell
aws --version
aws login --profile nexusoms-demo --region ap-southeast-1
```

Hoàn tất đăng nhập trong trình duyệt do AWS CLI mở.

## 2. Kiểm tra tài khoản

```powershell
$AwsProfile = "nexusoms-demo"
$AwsRegion = "ap-southeast-1"

aws sts get-caller-identity `
  --profile $AwsProfile `
  --region $AwsRegion
```

Đối chiếu `Account` với AWS Account ID trên Console.

## 3. Kiểm tra Availability Zone

```powershell
aws ec2 describe-availability-zones `
  --profile $AwsProfile `
  --region $AwsRegion `
  --query "AvailabilityZones[].ZoneName" `
  --output table
```

## 4. Lấy CloudFront managed prefix list ID

```powershell
$CloudFrontPrefixListId = aws ec2 describe-managed-prefix-lists --profile $AwsProfile --region $AwsRegion --filters "Name=prefix-list-name,Values=com.amazonaws.global.cloudfront.origin-facing" --query "PrefixLists[0].PrefixListId" --output text
$CloudFrontPrefixListId
```

![Kiểm tra tài khoản và CloudFront prefix list](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/16-get-available-zone-cloudfront.png)

Kết quả cần đạt:

- STS trả về đúng AWS Account ID.
- Region sử dụng là `ap-southeast-1`.
- Prefix list ID bắt đầu bằng `pl-`.
