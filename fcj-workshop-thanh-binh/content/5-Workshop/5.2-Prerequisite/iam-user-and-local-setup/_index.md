---
title: "IAM user and AWS CLI"
date: 2026-11-15
weight: 5
chapter: false
---

Create a lab IAM user and configure the `nexusoms-demo` profile.

## 1. Create the IAM user

Open IAM → **Users → Create user**.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/1-open-iam-user-dashboard.png" title="Open IAM Users" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/2-create-iam-user-button.png" title="Select Create user" >}}

```text
nexusoms-demo-deployer
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/3-configure-specify-user-detail.png" title="Enter the IAM user name" >}}

Select the lab permissions and create the user.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/4-configure-permission.png" title="Open permissions" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/5-select-permissions-policies.png" title="Select the permissions policy" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/6-click-create-user.png" title="Create the IAM user" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/7-complete-create-iam-user.png" title="IAM user created" >}}

## 2. Create the access key

Open the user → **Security credentials → Create access key** and select **Command Line Interface (CLI)**.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/8-configure-security-credentials-for-user.png" title="Open Security credentials" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/9-create-access-key.png" title="Create an access key" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/10-configure-command-link-interface-i-understand-next.png" title="Select Command Line Interface" >}}

```text
nexusoms-demo-powershell
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/11-set-description-tag.png" title="Enter the description" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/12-copy-access-key-download-csv.png" title="Save the access key" >}}

## 3. Configure PowerShell

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/13-open-powershell.png" title="Open PowerShell" >}}

```powershell
aws --version
aws configure --profile nexusoms-demo
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/14-check-aws-version-configure-profile.png" title="Configure the AWS profile" >}}

```text
AWS Access Key ID: <AWS_ACCESS_KEY_ID>
AWS Secret Access Key: <AWS_SECRET_ACCESS_KEY>
Default region name: ap-southeast-1
Default output format: json
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/15-configure-more.png" title="Complete aws configure" >}}

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

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/16-get-available-zone-cloudfront.png" title="Verify the AWS account and CloudFront prefix list" >}}

```powershell
git --version
docker --version
node --version
npm --version
dotnet --version
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/17-checking-library-in-cmd.png" title="Verify local tools" >}}
