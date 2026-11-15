---
title: "IAM user và AWS CLI"
date: 2026-11-15
weight: 5
chapter: false
---

Tạo IAM user cho bài lab và cấu hình profile `nexusoms-demo`.

## 1. Tạo IAM user

Mở IAM → **Users → Create user**.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/1-open-iam-user-dashboard.png" title="Mở IAM Users" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/2-create-iam-user-button.png" title="Chọn Create user" >}}

Nhập user name:

```text
nexusoms-demo-deployer
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/3-configure-specify-user-detail.png" title="Nhập IAM user name" >}}

Chọn permissions dùng cho tài khoản lab rồi tạo user.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/4-configure-permission.png" title="Mở phần permissions" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/5-select-permissions-policies.png" title="Chọn permissions policy" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/6-click-create-user.png" title="Tạo IAM user" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/7-complete-create-iam-user.png" title="IAM user đã được tạo" >}}

## 2. Tạo access key

Mở user → **Security credentials → Create access key**.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/8-configure-security-credentials-for-user.png" title="Mở Security credentials" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/9-create-access-key.png" title="Tạo access key" >}}

Chọn **Command Line Interface (CLI)**.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/10-configure-command-link-interface-i-understand-next.png" title="Chọn Command Line Interface" >}}

Nhập description:

```text
nexusoms-demo-powershell
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/11-set-description-tag.png" title="Nhập description" >}}

Lưu Access Key ID và Secret Access Key. Không đưa hai giá trị này lên GitHub.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/12-copy-access-key-download-csv.png" title="Lưu access key" >}}

## 3. Cấu hình PowerShell

Mở PowerShell.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/13-open-powershell.png" title="Mở PowerShell" >}}

Kiểm tra AWS CLI rồi bắt đầu cấu hình profile:

```powershell
aws --version
aws configure --profile nexusoms-demo
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/14-check-aws-version-configure-profile.png" title="Kiểm tra AWS CLI và cấu hình profile" >}}

Nhập lần lượt các giá trị khi PowerShell yêu cầu:

```text
AWS Access Key ID: <AWS_ACCESS_KEY_ID>
AWS Secret Access Key: <AWS_SECRET_ACCESS_KEY>
Default region name: ap-southeast-1
Default output format: json
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/15-configure-more.png" title="Hoàn tất aws configure" >}}

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

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/16-get-available-zone-cloudfront.png" title="Kiểm tra tài khoản và CloudFront prefix list" >}}

Kiểm tra các công cụ local:

```powershell
git --version
docker --version
node --version
npm --version
dotnet --version
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/17-checking-library-in-cmd.png" title="Kiểm tra công cụ local" >}}

Kết quả cần đạt: `get-caller-identity` trả về đúng AWS Account ID và lệnh prefix list trả về một ID bắt đầu bằng `pl-`.
