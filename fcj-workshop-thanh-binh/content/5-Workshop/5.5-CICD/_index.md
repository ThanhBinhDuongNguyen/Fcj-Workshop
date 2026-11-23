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

AWS STS validates the OIDC token against the IAM role trust policy. When `aud` and `sub` match, STS returns temporary credentials to the workflow. Do not create a separate STS resource.

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
$GitHubRepository = "nexus-aws"
$ViewerPermission = gh repo view "$GitHubOwner/$GitHubRepository" --json viewerPermission --jq .viewerPermission
if ($ViewerPermission -ne "ADMIN") { throw "The fork must return viewerPermission=ADMIN" }

$OidcCustomization = gh api "repos/$GitHubOwner/$GitHubRepository/actions/oidc/customization/sub" |
  ConvertFrom-Json
$OidcCustomization | ConvertTo-Json

if ($OidcCustomization.use_immutable_subject -and $OidcCustomization.sub_claim_prefix) {
  $OidcSubject = "$($OidcCustomization.sub_claim_prefix):ref:refs/heads/main"
} else {
  $OidcSubject = "repo:${GitHubOwner}/${GitHubRepository}:ref:refs/heads/main"
}

"VIEWER_PERMISSION=$ViewerPermission"
"OIDC_SUBJECT=$OidcSubject"
```

Copy the exact `OIDC_SUBJECT` value into the trust policy. If the workflow later uses a GitHub Environment, read the OIDC configuration again and replace the `sub` condition with the environment subject.

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

Check the branch and workflow files first:

```powershell
git status
git remote -v
git fetch origin --prune
git log --oneline --decorate -5
Get-ChildItem .github/workflows -File | Select-Object Name
$LegacyWorkflow = ".github/workflows/cd.yml"
if (Test-Path $LegacyWorkflow) { git rm $LegacyWorkflow }
Get-ChildItem .github/workflows -File | Where-Object Name -Match '^(cd|deploy)' | Select-Object Name
git diff -- .github/workflows
```

Only `deploy-aws.yml` may remain as the active CD workflow. Both CI workflows must support `workflow_call`, and their path filters must include `.github/workflows/**`.

The workshop repository already contains:

```text
.github/workflows/backend-ci.yml
.github/workflows/frontend-ci.yml
.github/workflows/deploy-aws.yml
```

`deploy-aws.yml` runs both CI workflows, deploys the backend to ECR/ECS, and deploys the frontend to S3/CloudFront. Open the file and verify its content.

Verify these three details in the tested workflow:

```text
Image tag: <sha>-<run_id>-<run_attempt>
Task definition source: the revision currently used by the ECS service
CloudFront: wait until the invalidation is completed
```

The **Verify assumed AWS role** step runs `aws sts get-caller-identity`. It must return the intended AWS account and the `nexusoms-demo-github-actions-role` ARN.

![Open the deployment workflow](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/13-edit-deployment-workflow.png)

![Update the deployment workflow](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/14-paste-deployment-workflow.png)

```powershell
git switch -c ci/aws-deployment
git add .github/workflows/backend-ci.yml .github/workflows/frontend-ci.yml .github/workflows/deploy-aws.yml
git diff --cached --stat
git commit -m "ci: deploy NexusOMS to AWS with GitHub OIDC"
git push -u origin ci/aws-deployment
gh pr create --base main --head ci/aws-deployment --title "ci: deploy NexusOMS to AWS" --body "Add the verified AWS deployment workflow."
```

Wait for Backend CI and Frontend CI to pass before merging. Do not use `git add -A` for this change.

After merging, update `main` and verify the workflow files again:

```powershell
git switch main
git pull --ff-only origin main
Get-ChildItem .github/workflows -File | Where-Object Name -Match '^(cd|deploy)' | Select-Object Name
git status
```

Only `deploy-aws.yml` should appear in the CD group, and the working tree must be clean.

If the workflow file did not change, open GitHub → **Actions → Deploy NexusOMS to AWS → Run workflow**.

Open GitHub → **Actions → Deploy NexusOMS to AWS** and wait for every job to pass.

A gray Docker build job on `main` is skipped by its condition, not failed. For a red job, open the first failed step before rerunning the workflow.

![GitHub Actions deployment succeeded](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/15-github-actions-deployment-success.png)

![The frontend bucket was updated](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/16-check-updated-frontend-bucket.png)
