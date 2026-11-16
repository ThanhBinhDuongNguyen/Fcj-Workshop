---
title: "IAM user and AWS CLI"
date: 2026-11-15
weight: 5
chapter: false
---

Create a lab IAM user and configure the `nexusoms-demo` profile.

## 1. Create the IAM user

Open IAM → **Users → Create user**.

![Open IAM Users](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/1-open-iam-user-dashboard.png)

![Select Create user](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/2-create-iam-user-button.png)

```text
nexusoms-demo-deployer
```

![Enter the IAM user name](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/3-configure-specify-user-detail.png)

Select the lab permissions and create the user.

![Open permissions](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/4-configure-permission.png)

![Select the permissions policy](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/5-select-permissions-policies.png)

![Create the IAM user](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/6-click-create-user.png)

![IAM user created](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/7-complete-create-iam-user.png)

## 2. Create the access key

Open the user → **Security credentials → Create access key** and select **Command Line Interface (CLI)**.

![Open Security credentials](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/8-configure-security-credentials-for-user.png)

![Create an access key](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/9-create-access-key.png)

![Select Command Line Interface](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/10-configure-command-link-interface-i-understand-next.png)

```text
nexusoms-demo-powershell
```

![Enter the description](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/11-set-description-tag.png)

![Save the access key](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/12-copy-access-key-download-csv.png)

## 3. Configure PowerShell

![Open PowerShell](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/13-open-powershell.png)

```powershell
aws --version
aws configure --profile nexusoms-demo
```

![Configure the AWS profile](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/14-check-aws-version-configure-profile.png)

```text
AWS Access Key ID: <AWS_ACCESS_KEY_ID>
AWS Secret Access Key: <AWS_SECRET_ACCESS_KEY>
Default region name: ap-southeast-1
Default output format: json
```

![Complete aws configure](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/15-configure-more.png)

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

![Verify the AWS account and CloudFront prefix list](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/16-get-available-zone-cloudfront.png)

```powershell
git --version
docker --version
node --version
npm --version
dotnet --version
```

![Verify local tools](/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/17-checking-library-in-cmd.png)
