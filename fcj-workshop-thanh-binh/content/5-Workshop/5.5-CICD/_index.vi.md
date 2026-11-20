---
title: "CI/CD với GitHub Actions"
date: 2026-11-15
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
---

Chỉ thực hiện phần này sau khi triển khai thủ công đã chạy end-to-end. CI/CD thay thế việc build/push/deploy thủ công cho các lần cập nhật, không thay thế bước tạo hạ tầng ban đầu.

## 1. Tạo GitHub OIDC provider trong AWS

IAM → **Identity providers → Add provider**:

![Mở Identity providers](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/1-open-identity-providers.png)

```text
Provider type: OpenID Connect
Provider URL: https://token.actions.githubusercontent.com
Audience: sts.amazonaws.com
```

![Cấu hình GitHub OIDC provider](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/2-configure-github-oidc-provider.png)



## 2. Tạo IAM role cho GitHub Actions

IAM → **Roles → Create role → Web identity**:

![Tạo IAM role](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/3-create-github-actions-role.png)

```text
Identity provider: token.actions.githubusercontent.com
Audience: sts.amazonaws.com
GitHub organization: <GITHUB_OWNER>
GitHub repository: <GITHUB_REPOSITORY>
GitHub branch: main
```

![Cấu hình trusted entity](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/4-configure-trusted-entity.png)

![Giới hạn repository và branch](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/5-limit-github-repository-and-branch.png)

Chọn **Next**. Tại **Add permissions**, chọn **Create inline policy → JSON** và dùng policy bên dưới.

Đọc cấu hình OIDC subject của repository trước khi tạo trust policy:

```powershell
gh auth status

$GitHubOwner = "<GITHUB_OWNER>"
$GitHubRepository = "nexus-aws"
$ViewerPermission = gh repo view "$GitHubOwner/$GitHubRepository" --json viewerPermission --jq .viewerPermission
if ($ViewerPermission -ne "ADMIN") { throw "Repository fork phải có viewerPermission=ADMIN" }

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

Sao chép chính xác giá trị `OIDC_SUBJECT` vào trust policy. Không tự dùng subject của repository khác. Nếu workflow chuyển sang GitHub Environment, đọc lại cấu hình OIDC và thay condition `sub` bằng subject của environment.

![Lấy GitHub owner ID và repository ID bằng PowerShell](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/5a-get-github-owner-and-repository-id.png)

Nếu `gh auth status` chưa đăng nhập, chạy `gh auth login` rồi thực hiện lại.

Sau khi tạo role, mở **Trust relationships → Edit trust policy**. Thay `<ACCOUNT_ID>` và `<OIDC_SUBJECT>`:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::<ACCOUNT_ID>:oidc-provider/token.actions.githubusercontent.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "token.actions.githubusercontent.com:aud": "sts.amazonaws.com",
          "token.actions.githubusercontent.com:sub": "<OIDC_SUBJECT>"
        }
      }
    }
  ]
}
```

Trong inline policy, thay `<ACCOUNT_ID>` và `<DISTRIBUTION_ID>` bằng giá trị thật:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "EcrLogin",
      "Effect": "Allow",
      "Action": "ecr:GetAuthorizationToken",
      "Resource": "*"
    },
    {
      "Sid": "PushBackendImage",
      "Effect": "Allow",
      "Action": [
        "ecr:BatchCheckLayerAvailability",
        "ecr:BatchGetImage",
        "ecr:CompleteLayerUpload",
        "ecr:GetDownloadUrlForLayer",
        "ecr:InitiateLayerUpload",
        "ecr:PutImage",
        "ecr:UploadLayerPart"
      ],
      "Resource": "arn:aws:ecr:ap-southeast-1:<ACCOUNT_ID>:repository/nexusoms-demo-api"
    },
    {
      "Sid": "RegisterTaskDefinition",
      "Effect": "Allow",
      "Action": [
        "ecs:DescribeTaskDefinition",
        "ecs:RegisterTaskDefinition"
      ],
      "Resource": "*"
    },
    {
      "Sid": "DeployEcsService",
      "Effect": "Allow",
      "Action": [
        "ecs:UpdateService",
        "ecs:DescribeServices"
      ],
      "Resource": "arn:aws:ecs:ap-southeast-1:<ACCOUNT_ID>:service/nexusoms-demo-cluster/nexusoms-demo-api-service"
    },
    {
      "Sid": "PassEcsRoles",
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": [
        "arn:aws:iam::<ACCOUNT_ID>:role/nexusoms-demo-ecs-execution-role",
        "arn:aws:iam::<ACCOUNT_ID>:role/nexusoms-demo-ecs-task-role"
      ],
      "Condition": {
        "StringEquals": {
          "iam:PassedToService": "ecs-tasks.amazonaws.com"
        }
      }
    },
    {
      "Sid": "ListFrontendBucket",
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetBucketLocation"
      ],
      "Resource": "arn:aws:s3:::nexusoms-demo-frontend-<ACCOUNT_ID>"
    },
    {
      "Sid": "DeployFrontendObjects",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::nexusoms-demo-frontend-<ACCOUNT_ID>/*"
    },
    {
      "Sid": "InvalidateCloudFront",
      "Effect": "Allow",
      "Action": [
        "cloudfront:CreateInvalidation",
        "cloudfront:GetInvalidation"
      ],
      "Resource": "arn:aws:cloudfront::<ACCOUNT_ID>:distribution/<DISTRIBUTION_ID>"
    }
  ]
}
```

Lưu inline policy với tên:

```text
DeployNexusOmsFromGitHub
```

Role name:

```text
nexusoms-demo-github-actions-role
```

![Tạo inline deployment policy](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/6-create-inline-deployment-policy.png)






## 3. GitHub repository variables và secrets

GitHub → repository → **Settings → Secrets and variables → Actions**.

![Mở Actions variables](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/7-open-actions-variables.png)

Tab **Variables** → phần **Repository variables** → **New repository variable**. Không dùng phần **Environment variables**.

Variables:

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

![Tạo repository variable](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/8-create-repository-variable.png)

![Hoàn tất repository variables](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/9-complete-repository-variables.png)

Tab **Secrets** → phần **Repository secrets** → **New repository secret**.

![Mở repository secrets](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/10-open-repository-secrets.png)

Secrets:

```text
AWS_ROLE_TO_ASSUME=arn:aws:iam::<ACCOUNT_ID>:role/nexusoms-demo-github-actions-role
VITE_SUPABASE_URL=https://<PROJECT_REF>.supabase.co
VITE_SUPABASE_ANON_KEY=<SUPABASE_PUBLISHABLE_OR_ANON_KEY>
```

![Tạo repository secret](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/11-create-repository-secret.png)

![Hoàn tất repository secrets](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/12-complete-repository-secrets.png)

Dùng Supabase publishable/anon key cho `VITE_SUPABASE_ANON_KEY`.







## 4. Workflow hợp nhất

Kiểm tra branch và workflow trước khi chỉnh:

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

Danh sách workflow CD chỉ được còn `deploy-aws.yml`. `backend-ci.yml` và `frontend-ci.yml` phải có `workflow_call` và path filter phải bao gồm `.github/workflows/**` để thay đổi workflow vẫn chạy CI.

Repository thực hành đã có ba workflow:

```text
.github/workflows/backend-ci.yml
.github/workflows/frontend-ci.yml
.github/workflows/deploy-aws.yml
```

`deploy-aws.yml` gọi lại hai workflow CI, sau đó deploy backend lên ECR/ECS và frontend lên S3/CloudFront. Mở file để kiểm tra nội dung:

![Mở deployment workflow](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/13-edit-deployment-workflow.png)

```yaml
name: Deploy NexusOMS to AWS

on:
  workflow_dispatch:
  push:
    branches: [main]

permissions:
  contents: read
  id-token: write

concurrency:
  group: nexusoms-production
  cancel-in-progress: false

jobs:
  backend-ci:
    uses: ./.github/workflows/backend-ci.yml

  frontend-ci:
    uses: ./.github/workflows/frontend-ci.yml

  deploy-backend:
    needs: [backend-ci, frontend-ci]
    runs-on: ubuntu-latest
    outputs:
      image: ${{ steps.build-image.outputs.image }}
    steps:
      - name: Checkout source
        uses: actions/checkout@v4

      - name: Configure AWS credentials by OIDC
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_ROLE_TO_ASSUME }}
          aws-region: ${{ vars.AWS_REGION }}

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build and push immutable backend image
        id: build-image
        env:
          REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          IMAGE_TAG: ${{ github.sha }}-${{ github.run_id }}-${{ github.run_attempt }}
        run: |
          IMAGE="$REGISTRY/${{ vars.ECR_REPOSITORY }}:$IMAGE_TAG"
          docker build --file backend/Dockerfile --tag "$IMAGE" backend
          docker push "$IMAGE"
          echo "image=$IMAGE" >> "$GITHUB_OUTPUT"

      - name: Download and sanitize current ECS task definition
        run: |
          TASK_DEFINITION_ARN=$(aws ecs describe-services \
            --cluster "${{ vars.ECS_CLUSTER }}" \
            --services "${{ vars.ECS_SERVICE }}" \
            --query "services[0].taskDefinition" \
            --output text)
          aws ecs describe-task-definition \
            --task-definition "$TASK_DEFINITION_ARN" \
            --query taskDefinition > task-definition.raw.json
          jq 'del(
            .taskDefinitionArn,
            .revision,
            .status,
            .requiresAttributes,
            .compatibilities,
            .registeredAt,
            .registeredBy
          )' task-definition.raw.json > task-definition.json

      - name: Render task definition with new image
        id: render-task
        uses: aws-actions/amazon-ecs-render-task-definition@v1
        with:
          task-definition: task-definition.json
          container-name: ${{ vars.CONTAINER_NAME }}
          image: ${{ steps.build-image.outputs.image }}

      - name: Deploy ECS task definition
        uses: aws-actions/amazon-ecs-deploy-task-definition@v2
        with:
          task-definition: ${{ steps.render-task.outputs.task-definition }}
          service: ${{ vars.ECS_SERVICE }}
          cluster: ${{ vars.ECS_CLUSTER }}
          wait-for-service-stability: true

  deploy-frontend:
    needs: deploy-backend
    runs-on: ubuntu-latest
    steps:
      - name: Checkout source
        uses: actions/checkout@v4

      - name: Configure AWS credentials by OIDC
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_ROLE_TO_ASSUME }}
          aws-region: ${{ vars.AWS_REGION }}

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm
          cache-dependency-path: frontend/package-lock.json

      - name: Install, check and build frontend
        working-directory: frontend
        env:
          VITE_DATA_MODE: supabase
          VITE_API_BASE_URL: https://${{ vars.CLOUDFRONT_DOMAIN }}
          VITE_SUPABASE_URL: ${{ secrets.VITE_SUPABASE_URL }}
          VITE_SUPABASE_ANON_KEY: ${{ secrets.VITE_SUPABASE_ANON_KEY }}
        run: |
          npm ci
          npm run typecheck
          npm run test
          npm run build

      - name: Upload frontend to private S3 bucket
        run: |
          aws s3 sync frontend/dist "s3://${{ vars.S3_BUCKET }}" --delete

      - name: Invalidate CloudFront cache
        run: |
          INVALIDATION_ID=$(aws cloudfront create-invalidation \
            --distribution-id "${{ vars.CLOUDFRONT_DISTRIBUTION_ID }}" \
            --paths "/*" \
            --query "Invalidation.Id" \
            --output text)
          aws cloudfront wait invalidation-completed \
            --distribution-id "${{ vars.CLOUDFRONT_DISTRIBUTION_ID }}" \
            --id "$INVALIDATION_ID"
```

![Cập nhật deployment workflow](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/14-paste-deployment-workflow.png)

Nếu vừa chỉnh workflow, tạo branch riêng và pull request:

```powershell
git switch -c ci/aws-deployment
git add .github/workflows/backend-ci.yml .github/workflows/frontend-ci.yml .github/workflows/deploy-aws.yml
git diff --cached --stat
git commit -m "ci: deploy NexusOMS to AWS with GitHub OIDC"
git push -u origin ci/aws-deployment
gh pr create --base main --head ci/aws-deployment --title "ci: deploy NexusOMS to AWS" --body "Add the verified AWS deployment workflow."
```

Mở pull request và chờ Backend CI cùng Frontend CI thành công rồi mới merge. Chỉ stage ba workflow; không dùng `git add -A`.

Sau khi merge, đồng bộ lại `main` và kiểm tra workflow lần cuối:

```powershell
git switch main
git pull --ff-only origin main
Get-ChildItem .github/workflows -File | Where-Object Name -Match '^(cd|deploy)' | Select-Object Name
git status
```

Kết quả cần đạt: chỉ có `deploy-aws.yml` trong nhóm CD và working tree sạch.

Nếu không chỉnh file, mở GitHub → **Actions → Deploy NexusOMS to AWS → Run workflow**.

## 5. Xác minh pipeline

Merge pull request vào `main` hoặc chọn **Run workflow** để chạy pipeline. Chờ các job:

```text
Backend CI ─┐
            ├─> Deploy backend ─> Deploy frontend
Frontend CI ┘
```

Kiểm tra:

1. ECR có image tag mới.
2. ECS có task definition revision mới.
3. ECS service giữ `Running 1/1` và target `Healthy`.
4. Frontend được đồng bộ vào S3.
5. CloudFront invalidation có trạng thái `Completed`.
6. Website đăng nhập và gọi API thành công.

Job Docker build hiển thị màu xám trên lần chạy `main` là **skipped** theo điều kiện, không phải lỗi. Nếu có job đỏ, mở đúng job và đọc bước đầu tiên thất bại trước khi chạy lại.

![GitHub Actions triển khai thành công](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/15-github-actions-deployment-success.png)

![Frontend bucket đã được cập nhật](/images/5-Workshop/nexusoms/5.5-CICD/github-actions/16-check-updated-frontend-bucket.png)

Nếu pipeline lỗi:

| Lỗi | Kiểm tra |
|---|---|
| `Not authorized to perform sts:AssumeRoleWithWebIdentity` | Trust policy phải dùng đúng immutable `<OIDC_SUBJECT>` |
| ECR trả `403 Forbidden` khi kiểm tra manifest | Inline policy phải có `ecr:BatchGetImage` |

Rollback backend:

1. ECS → service → Update.
2. Chọn task definition revision trước đó.
3. Force new deployment.

Rollback frontend: checkout commit ổn định và chạy lại workflow, hoặc upload lại bundle đã lưu rồi invalidate CloudFront.

---
