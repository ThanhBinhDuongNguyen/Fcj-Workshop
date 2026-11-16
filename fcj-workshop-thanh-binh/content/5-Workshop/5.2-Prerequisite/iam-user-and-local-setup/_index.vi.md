---
title: "IAM user và AWS CLI"
date: 2026-11-15
weight: 5
chapter: false
---

Tạo IAM user cho bài lab và cấu hình profile `nexusoms-demo`.

## 1. Tạo IAM user

Mở IAM → **Users → Create user**.

![Mở IAM Users](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/1-open-iam-user-dashboard.png)

![Chọn Create user](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/2-create-iam-user-button.png)

Nhập user name:

```text
nexusoms-demo-deployer
```

![Nhập IAM user name](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/3-configure-specify-user-detail.png)

Chọn permissions dùng cho tài khoản lab rồi tạo user.

![Mở phần permissions](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/4-configure-permission.png)

![Chọn permissions policy](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/5-select-permissions-policies.png)

![Tạo IAM user](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/6-click-create-user.png)

![IAM user đã được tạo](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/7-complete-create-iam-user.png)

## 2. Tạo access key

Mở user → **Security credentials → Create access key**.

![Mở Security credentials](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/8-configure-security-credentials-for-user.png)

![Tạo access key](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/9-create-access-key.png)

Chọn **Command Line Interface (CLI)**.

![Chọn Command Line Interface](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/10-configure-command-link-interface-i-understand-next.png)

Nhập description:

```text
nexusoms-demo-powershell
```

![Nhập description](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/11-set-description-tag.png)

Lưu Access Key ID và Secret Access Key. Không đưa hai giá trị này lên GitHub.

![Lưu access key](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/12-copy-access-key-download-csv.png)

## 3. Cấu hình PowerShell

Mở PowerShell.

![Mở PowerShell](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/13-open-powershell.png)

Kiểm tra AWS CLI rồi bắt đầu cấu hình profile:

```powershell
aws --version
aws configure --profile nexusoms-demo
```

![Kiểm tra AWS CLI và cấu hình profile](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/14-check-aws-version-configure-profile.png)

Nhập lần lượt các giá trị khi PowerShell yêu cầu:

```text
AWS Access Key ID: <AWS_ACCESS_KEY_ID>
AWS Secret Access Key: <AWS_SECRET_ACCESS_KEY>
Default region name: ap-southeast-1
Default output format: json
```

![Hoàn tất aws configure](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/15-configure-more.png)

Kiểm tra tài khoản, Availability Zone và lấy CloudFront managed prefix list ID:

```powershell
$env:AWS_PROFILE = "nexusoms-demo"

aws sts get-caller-identity `
  --profile nexusoms-demo `
  --region ap-southeast-1

aws ec2 describe-availability-zones `
  --profile nexusoms-demo `
  --region ap-southeast-1 `
  --query "AvailabilityZones[].ZoneName" `
  --output table

aws ec2 describe-managed-prefix-lists `
  --profile nexusoms-demo `
  --region ap-southeast-1 `
  --query "PrefixLists[?PrefixListName=='com.amazonaws.global.cloudfront.origin-facing'].PrefixListId" `
  --output text
```

Ghi lại kết quả `pl-...` để tạo ALB Security Group.

![Kiểm tra tài khoản và CloudFront prefix list](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/16-get-available-zone-cloudfront.png)

Kiểm tra các công cụ local:

```powershell
git --version
docker --version
node --version
npm --version
dotnet --version
```

![Kiểm tra công cụ local](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/17-checking-library-in-cmd.png)

Kết quả cần đạt: `get-caller-identity` trả về đúng AWS Account ID và lệnh prefix list trả về một ID bắt đầu bằng `pl-`.
