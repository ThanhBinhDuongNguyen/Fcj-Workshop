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

![Open Identity providers](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/1-open-identity-providers.png)

![Configure the GitHub OIDC provider](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/2-configure-github-oidc-provider.png)

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

![Create the IAM role](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/3-create-github-actions-role.png)

![Configure the trusted entity](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/4-configure-trusted-entity.png)

![Limit the repository and branch](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/5-limit-github-repository-and-branch.png)

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

![Get the GitHub owner ID and repository ID with PowerShell](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/5a-get-github-owner-and-repository-id.png)

![Create the deployment policy](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/6-create-inline-deployment-policy.png)

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

![Open Actions variables](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/7-open-actions-variables.png)

![Create a repository variable](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/8-create-repository-variable.png)

![Complete repository variables](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/9-complete-repository-variables.png)

![Open repository secrets](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/10-open-repository-secrets.png)

![Create a repository secret](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/11-create-repository-secret.png)

![Complete repository secrets](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/12-complete-repository-secrets.png)

## 4. Run the unified workflow

The workshop repository already contains:

```text
.github/workflows/backend-ci.yml
.github/workflows/frontend-ci.yml
.github/workflows/deploy-aws.yml
```

`deploy-aws.yml` runs both CI workflows, deploys the backend to ECR/ECS, and deploys the frontend to S3/CloudFront. Open the file and verify its content.

![Open the deployment workflow](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/13-edit-deployment-workflow.png)

![Update the deployment workflow](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/14-paste-deployment-workflow.png)

```powershell
git add -A .github/workflows
git commit -m "ci: deploy NexusOMS to AWS with GitHub OIDC"
git push origin main
```

If the workflow file did not change, open GitHub → **Actions → Deploy NexusOMS to AWS → Run workflow**.

Open GitHub → **Actions → Deploy NexusOMS to AWS** and wait for every job to pass.

![GitHub Actions deployment succeeded](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/15-github-actions-deployment-success.png)

![The frontend bucket was updated](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/16-check-updated-frontend-bucket.png)
