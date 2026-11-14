---
title: "Accounts and local environment"
date: 2026-11-13
weight: 1
chapter: false
---

Complete this section before creating the VPC. Never commit access keys, database passwords, or secrets to Git.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## Values to enter or copy

> Copy the matching line into the input field. Replace every `<...>` placeholder with the real value from your AWS or Supabase account.

**PowerShell command**

```text
git clone https://github.com/ThanhBinhDuongNguyen/nexus-aws.git
```

**PowerShell command**

```text
cd nexus-aws
```

**Region**

```text
ap-southeast-1
```

**Accounts and local environment - input 4**

```text
nexusoms-demo-deployer
```

**Accounts and local environment - input 5**

```text
nexusoms-demo-powershell
```

**PowerShell command**

```text
aws configure --profile nexusoms-demo
```

**PowerShell command**

```text
$env:AWS_PROFILE = 'nexusoms-demo'
```

**PowerShell command**

```text
aws sts get-caller-identity --profile nexusoms-demo --region ap-southeast-1
```

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Open IAM user dashboard

In AWS Console, complete **Open IAM user dashboard** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/1-open-iam-user-dashboard.png" title="Open IAM user dashboard" >}}

### Step 2 - Create IAM user button

In AWS Console, complete **Create IAM user button** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/2-create-iam-user-button.png" title="Create IAM user button" >}}

### Step 3 - Configure Specify user detail

In AWS Console, complete **Configure Specify user detail** as shown below.

**Values used in this step**

**Region**

```text
ap-southeast-1
```

**Accounts and local environment - input 4**

```text
nexusoms-demo-deployer
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/3-configure-specify-user-detail.png" title="Configure Specify user detail" >}}

### Step 4 - Configure permission

In AWS Console, complete **Configure permission** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/4-configure-permission.png" title="Configure permission" >}}

### Step 5 - Select Permissions Policies

In AWS Console, complete **Select Permissions Policies** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/5-select-permissions-policies.png" title="Select Permissions Policies" >}}

### Step 6 - Click Create User

In AWS Console, complete **Click Create User** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/6-click-create-user.png" title="Click Create User" >}}

### Step 7 - Complete create IAM User

In AWS Console, complete **Complete create IAM User** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/7-complete-create-iam-user.png" title="Complete create IAM User" >}}

### Step 8 - Configure Security credentials for user

In AWS Console, complete **Configure Security credentials for user** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/8-configure-security-credentials-for-user.png" title="Configure Security credentials for user" >}}

### Step 9 - Create access key

In AWS Console, complete **Create access key** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/9-create-access-key.png" title="Create access key" >}}

### Step 10 - Configure Command Link Interface + I understand... + Next

In AWS Console, complete **Configure Command Link Interface + I understand... + Next** as shown below.

**Values used in this step**

**PowerShell command**

```text
git clone https://github.com/ThanhBinhDuongNguyen/nexus-aws.git
```

**PowerShell command**

```text
cd nexus-aws
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/10-configure-command-link-interface-i-understand-next.png" title="Configure Command Link Interface + I understand... + Next" >}}

### Step 11 - Set description tag

In AWS Console, complete **Set description tag** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/11-set-description-tag.png" title="Set description tag" >}}

### Step 12 - Copy access key + download csv

In AWS Console, complete **Copy access key + download csv** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/12-copy-access-key-download-csv.png" title="Copy access key + download csv" >}}

### Step 13 - Open PowerShell

In AWS Console, complete **Open PowerShell** as shown below.

**Values used in this step**

**Accounts and local environment - input 5**

```text
nexusoms-demo-powershell
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/13-open-powershell.png" title="Open PowerShell" >}}

### Step 14 - Check AWS version + Configure profile

In AWS Console, complete **Check AWS version + Configure profile** as shown below.

**Values used in this step**

**PowerShell command**

```text
aws configure --profile nexusoms-demo
```

**PowerShell command**

```text
$env:AWS_PROFILE = 'nexusoms-demo'
```

**PowerShell command**

```text
aws sts get-caller-identity --profile nexusoms-demo --region ap-southeast-1
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/14-check-aws-version-configure-profile.png" title="Check AWS version + Configure profile" >}}

### Step 15 - Configure more

In AWS Console, complete **Configure more** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/15-configure-more.png" title="Configure more" >}}

### Step 16 - Get available zone + CloudFront

In AWS Console, complete **Get available zone + CloudFront** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/16-get-available-zone-cloudfront.png" title="Get available zone + CloudFront" >}}

### Step 17 - Checking library in cmd

In AWS Console, complete **Checking library in cmd** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/17-checking-library-in-cmd.png" title="Checking library in cmd" >}}

### Step 18 - Run test for BE

In AWS Console, complete **Run test for BE** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/18-1-run-test-for-be.png" title="Run test for BE" >}}

### Step 19 - Docker build project

In AWS Console, complete **Docker build project** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/18-2-docker-build-project.png" title="Docker build project" >}}

### Step 20 - CI & type-check FE

In AWS Console, complete **CI & type-check FE** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/19-1-ci-type-check-fe.png" title="CI & type-check FE" >}}

### Step 21 - Run test + run build FE

In AWS Console, complete **Run test + run build FE** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/19-2-run-test-run-build-fe.png" title="Run test + run build FE" >}}
