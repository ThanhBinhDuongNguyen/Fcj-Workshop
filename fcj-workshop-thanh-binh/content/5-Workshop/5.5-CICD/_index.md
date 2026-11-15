---
title: "GitHub Actions CI/CD"
date: 2026-11-15
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
---

Run this section only after the manual deployment is healthy.

## 1. Add the GitHub OIDC provider

IAM → **Identity providers → Add provider**:

```text
Provider type: OpenID Connect
Provider URL: https://token.actions.githubusercontent.com
Audience: sts.amazonaws.com
```

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/1-open-identity-providers.png" title="Open Identity providers" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/2-configure-github-oidc-provider.png" title="Configure the GitHub OIDC provider" >}}

## 2. Create the GitHub Actions role

IAM → **Roles → Create role → Web identity**:

```text
Identity provider: token.actions.githubusercontent.com
Audience: sts.amazonaws.com
GitHub organization: <GITHUB_OWNER>
GitHub repository: <GITHUB_REPOSITORY>
GitHub branch: main
Role name: nexusoms-demo-github-actions-role
```

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/3-create-github-actions-role.png" title="Create the IAM role" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/4-configure-trusted-entity.png" title="Configure the trusted entity" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/5-limit-github-repository-and-branch.png" title="Limit the repository and branch" >}}

```powershell
gh auth status

$GitHubOwner = "<GITHUB_OWNER>"
$GitHubRepository = "nexusoms-workshop"
$RepoInfo = gh api "repos/$GitHubOwner/$GitHubRepository" |
  ConvertFrom-Json
$GitHubOwnerId = $RepoInfo.owner.id
$GitHubRepositoryId = $RepoInfo.id
$OidcSubject = "repo:${GitHubOwner}@${GitHubOwnerId}/${GitHubRepository}@${GitHubRepositoryId}:ref:refs/heads/main"
"OIDC_SUBJECT=$OidcSubject"
```

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/5a-get-github-owner-and-repository-id.png" title="Get the GitHub owner ID and repository ID with PowerShell" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/6-create-inline-deployment-policy.png" title="Create the deployment policy" >}}

Use the trust policy and `DeployNexusOmsFromGitHub` inline policy from the Vietnamese page. Replace every placeholder before saving.

## 3. Add repository variables and secrets

GitHub → repository → **Settings → Secrets and variables → Actions**.

Create these repository variables:

```text
AWS_REGION=ap-southeast-1
ECR_REPOSITORY=nexusoms-demo-api
ECS_CLUSTER=nexusoms-demo-cluster
ECS_SERVICE=nexusoms-demo-api-service
ECS_TASK_DEFINITION=nexusoms-demo-api
CONTAINER_NAME=api
S3_BUCKET=nexusoms-demo-frontend-<ACCOUNT_ID>
CLOUDFRONT_DISTRIBUTION_ID=<DISTRIBUTION_ID>
CLOUDFRONT_DOMAIN=<DOMAIN>.cloudfront.net
```

Create these repository secrets:

```text
AWS_ROLE_TO_ASSUME=arn:aws:iam::<ACCOUNT_ID>:role/nexusoms-demo-github-actions-role
VITE_SUPABASE_URL=https://<PROJECT_REF>.supabase.co
VITE_SUPABASE_ANON_KEY=<SUPABASE_PUBLISHABLE_OR_ANON_KEY>
```

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/7-open-actions-variables.png" title="Open Actions variables" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/8-create-repository-variable.png" title="Create a repository variable" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/9-complete-repository-variables.png" title="Complete repository variables" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/10-open-repository-secrets.png" title="Open repository secrets" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/11-create-repository-secret.png" title="Create a repository secret" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/12-complete-repository-secrets.png" title="Complete repository secrets" >}}

## 4. Run the unified workflow

Disable the old GHCR publishing workflow:

```powershell
if (Test-Path ".github/workflows/cd.yml") {
  Rename-Item ".github/workflows/cd.yml" "cd-ghcr.yml.disabled"
}
```

Use `.github/workflows/deploy-aws.yml`, then commit and push the workflow to `main`.

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/13-edit-deployment-workflow.png" title="Open the deployment workflow" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/14-paste-deployment-workflow.png" title="Update the deployment workflow" >}}

```powershell
git add -A .github/workflows
git commit -m "ci: deploy NexusOMS to AWS with GitHub OIDC"
git push origin main
```

Open GitHub → **Actions → Deploy NexusOMS to AWS** and wait for every job to pass.

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/15-github-actions-deployment-success.png" title="GitHub Actions deployment succeeded" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.5-CICD/github-actions/16-check-updated-frontend-bucket.png" title="The frontend bucket was updated" >}}
