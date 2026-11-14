---
title: "Tài khoản và môi trường local"
date: 2026-11-13
weight: 1
chapter: false
---

Hoàn thành toàn bộ phần này trước khi tạo VPC. Không đưa access key, database password hoặc secret vào Git.

## 1.1 Chuẩn bị mã nguồn và Supabase

## 1. Công cụ cần cài đặt

Người thực hành cần chuẩn bị:

- Git
- Node.js 22 và npm
- .NET SDK 10
- Docker Desktop
- Supabase CLI thông qua npx
- Một tài khoản Supabase
- Một tài khoản AWS nếu tiếp tục triển khai lên AWS

Kiểm tra:

```
git --version
node --version
npm --version
dotnet --version
docker version
```

## 2. Clone mã nguồn

```
git clone https://github.com/ThanhBinhDuongNguyen/nexus-aws.git
cd nexus-aws
```

Cấu trúc chính:

```
nexus-aws/
├── frontend/           React + Vite
├── backend/            ASP.NET Core API
├── supabase/           Database migrations
└── infra/              Cấu hình triển khai AWS
```

Không sử dụng .env.local, database password hoặc secret của người tạo Workshop. Mỗi người thực hành phải tạo Supabase riêng.

## 3. Tạo Supabase Project

1. Truy cập [Supabase Dashboard](https://supabase.com/dashboard).
2. Chọn New project.
3. Điền:
   - Project name: tùy chọn, ví dụ nexusoms-workshop
   - Database password: tạo mật khẩu mạnh và lưu lại
   - Region: chọn khu vực gần người dùng
4. Chờ project khởi tạo xong.

Ghi lại các giá trị sau:

| **Giá trị**                  | **Vị trí lấy**                |
| ---------------------------- | ----------------------------- |
| Project reference            | `Project Settings → General`  |
| Project URL                  | `Project Settings → API Keys` |
| Publishable/anon key         | `Project Settings → API Keys` |
| PostgreSQL connection string | Nút Connect → Session pooler  |

Không sử dụng service_role key hoặc secret key trong frontend.

## 4. Khởi tạo database bằng migrations

Tại thư mục gốc dự án:

```
npx supabase@latest login
```

Trình duyệt sẽ mở để xác nhận tài khoản Supabase.

Liên kết repository với project vừa tạo:

```
npx supabase@latest link --project-ref <PROJECT_REF>
```

Thay &lt;PROJECT_REF&gt; bằng Project reference lấy ở bước trước.

Kiểm tra migrations trước:

```
npx supabase@latest db push --dry-run --remote
```

Nếu danh sách migrations đúng, áp dụng:

```
npx supabase@latest db push --remote
```

Sau đó vào Supabase → Table Editor và kiểm tra các bảng như:

- profiles
- products
- warehouses
- inventory_items
- orders
- channel_connections

## 5. Tạo tài khoản đăng nhập

1. Vào Supabase → Authentication → Users.
2. Chọn Add user → Create new user.
3. Nhập email và mật khẩu dùng để đăng nhập NexusOMS.
4. Bật xác nhận email nếu giao diện yêu cầu.
5. Copy UUID của user vừa tạo.

Migration đã cài trigger tự tạo một bản ghi tương ứng trong public.profiles. Role mặc định là warehouse.

Để dùng đầy đủ chức năng demo, vào SQL Editor và chạy:

```
update public.profiles
set role = 'manager',
    is_active = true
where id = '<USER_UUID>';
```

Kiểm tra:

```
select id, email, full_name, role, is_active
from public.profiles
where id = '<USER_UUID>';
```

UUID này đồng thời được dùng làm WebhookSimulator:SystemActorId.

Các role hợp lệ:

- admin
- manager
- warehouse

## 6. Cấu hình Backend local

Lấy PostgreSQL connection string tại:

```
Supabase → Connect → Session pooler
```

Không tự đoán hostname hoặc username. Copy đúng thông tin Supabase cung cấp và thay database password.

Tại thư mục backend:

```
dotnet user-secrets set "ConnectionStrings:Postgres" "<POSTGRES_CONNECTION_STRING>" --project src/NexusOMS.Api
dotnet user-secrets set "Supabase:Url" "https://<PROJECT_REF>.supabase.co" --project src/NexusOMS.Api
dotnet user-secrets set "WebhookSimulator:SystemActorId" "<USER_UUID>" --project src/NexusOMS.Api
dotnet user-secrets set "WebhookSimulator:Secret" "<RANDOM_SECRET>" --project src/NexusOMS.Api
dotnet user-secrets set "Frontend:Origin" "http://localhost:5173" --project src/NexusOMS.Api
```

RANDOM_SECRET là chuỗi bí mật tự tạo, dài ít nhất 32 ký tự, ví dụ:

```
nexusoms-workshop-webhook-2026-x8Kp92Lm
```

Không commit chuỗi này lên GitHub.

Khởi động backend:

```
dotnet restore
dotnet run --project src/NexusOMS.Api --launch-profile http
```

Backend chạy tại:

```
http://localhost:5145
```

Kiểm tra:

```
http://localhost:5145/health
```

Kết quả mong đợi:

```
{
  "status": "ok"
}
```

## 7. Cấu hình Frontend local

Trong thư mục frontend, tạo file .env.local:

```
VITE_DATA_MODE=supabase
VITE_API_BASE_URL=http://localhost:5145
VITE_SUPABASE_URL=https://<PROJECT_REF>.supabase.co
VITE_SUPABASE_ANON_KEY=<SUPABASE_PUBLISHABLE_KEY>
```

Cài package và chạy frontend:

```
npm ci
npm run dev
```

Mở địa chỉ Vite hiển thị trên terminal, thông thường:

```
http://localhost:5173
```

Đăng nhập bằng user đã tạo ở Supabase.

## 8. Kiểm tra trước khi triển khai AWS

- Đăng nhập thành công.
- GET /api/v1/session/me trả về user và role.
- Trang sản phẩm, kho và đơn hàng tải được dữ liệu.
- Webhook simulator nhận UUID profile hợp lệ.
- File .env.local bị Git bỏ qua:

```
git check-ignore -v frontend/.env.local
```

## 2. Điền bảng thông tin triển khai

## 2.1. Ghi lại thông tin triển khai

Tạo một bản ghi riêng trong ghi chú cá nhân. Không điền secret vào tài liệu này.

| **Tên**                      | **Giá trị**                                                        | **Lấy chính xác ở đâu**                                                                                        |
| ---------------------------- | ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| AWS region                   | `ap-southeast-1`                                                   | Giá trị cố định; chọn Asia Pacific (Singapore) ở góc phải AWS Console                                          |
| AWS account ID               | 12 chữ số                                                          | PowerShell: aws sts get-caller-identity, lấy trường Account                                                    |
| Availability Zone A/B        | Hai AZ trong Singapore                                             | VPC → Subnets → Create subnet → menu Availability Zone; hoặc lệnh ở dưới. Dùng hai AZ khác nhau                |
| CloudFront prefix list ID    | Bắt đầu bằng pl-                                                   | Chạy lệnh ở dưới; chọn prefix-list-name com.amazonaws.global.cloudfront.origin-facing                          |
| Supabase project URL         | `https://<project-ref>.supabase.co`                                | Supabase → Project Settings → API → Project URL                                                                |
| Supabase publishable key     | Bắt đầu bằng sb_publishable_ hoặc key công khai được Dashboard cấp | Supabase → Project Settings → API Keys → Publishable key. Chỉ dùng lúc build frontend                          |
| PostgreSQL connection string | Chuỗi Npgsql dạng Host=...;Port=5432;...                           | Supabase Dashboard → Connect → Session pooler. Copy host và username; thay password bằng database password mới |
| System actor profile ID      | UUID                                                               | Supabase → SQL Editor, chạy truy vấn trong phần 7.3 và copy cột id đúng tài khoản                              |
| ECR repository URI           | URI kết thúc bằng /nexusoms-demo-api                               | ECR → Private registry → Repositories → mở repository → Copy URI                                               |
| Frontend bucket name         | Tên bucket thực tế                                                 | S3 → Buckets; copy đúng tên bucket frontend đã tạo                                                             |
| ALB DNS name                 | DNS kết thúc bằng .elb.amazonaws.com                               | EC2 → Load Balancers → chọn ALB → Details → DNS name                                                           |
| CloudFront domain            | Ví dụ dxxxxxxxxxxxx.cloudfront.net                                 | CloudFront → Distributions → chọn distribution → General → Distribution domain name                            |
| CloudFront Distribution ID   | Mã chữ/số của distribution                                         | Cùng trang General, trường Distribution ID; không dùng domain thay ID                                          |

## 2.2. Tạo IAM user tạm và kết nối PowerShell

Phần này tạo một IAM user tạm để AWS CLI trên máy Windows có quyền dựng workshop. Không dùng root account cho lệnh hằng ngày. User này chỉ phục vụ triển khai và phải được thu hồi sau demo.

**A.** Tạo IAM user triển khai tạm

1\. Đăng nhập AWS Console bằng tài khoản có quyền quản trị IAM; chọn region Asia Pacific Singapore ap-southeast-1.

2\. Mở IAM → Users → Create user.

3\. User name: nexusoms-demo-deployer.

4\. Không bật Provide user access to the AWS Management Console; user này chỉ dùng cho CLI.

5\. Chọn Attach policies directly.

6\. Tìm và chọn AWS managed policy AdministratorAccess.

7\. Chọn Create user.

Lý do dùng AdministratorAccess tạm: workshop phải tạo IAM, VPC, ECS, ECR, S3 và CloudFront. Quyền này rất rộng nên không được gắn cho ứng dụng, không ghi Access Key vào source code và phải gỡ sau demo.

**B.** Tạo Access Key cho AWS CLI

1\. Mở user nexusoms-demo-deployer → Security credentials.

2\. Trong Access keys, chọn Create access key.

3\. Chọn use case Command Line Interface CLI; xác nhận cảnh báo và chọn Next.

4\. Description tag value: nexusoms-demo-powershell; chọn Create access key.

5\. Copy Access key ID và Secret access key hoặc tải file CSV. Secret chỉ hiện một lần.

Không chụp màn hình secret, không gửi qua chat, không lưu vào .env hoặc repository. Nếu secret bị lộ, Deactivate và xóa Access Key ngay rồi tạo key mới.

**C.** Cấu hình profile AWS CLI trên PowerShell

```
aws --version
aws configure --profile nexusoms-demo
```

Nhập lần lượt các giá trị sau khi PowerShell hỏi:

| **Trường CLI**        | **Giá trị nhập**          | **Lấy ở đâu**                                    |
| --------------------- | ------------------------- | ------------------------------------------------ |
| AWS Access Key ID     | Access key ID vừa tạo     | IAM user → Security credentials → Access keys    |
| AWS Secret Access Key | Secret access key vừa tạo | Màn hình Create access key hoặc file CSV vừa tải |
| Default region name   | ap-southeast-1            | Region triển khai Singapore đã chốt              |
| Default output format | json                      | Định dạng đầu ra dễ kiểm tra trong PowerShell    |

**D.** Chọn profile và xác minh danh tính

```
$env:AWS_PROFILE = 'nexusoms-demo'
$env:AWS_REGION = 'ap-southeast-1'
aws configure list --profile nexusoms-demo
aws sts get-caller-identity --profile nexusoms-demo --region ap-southeast-1
```

Kết quả đúng: Account là AWS account ID của bạn; Arn kết thúc bằng user/nexusoms-demo-deployer; region là ap-southeast-1. Nếu gặp InvalidClientTokenId, SignatureDoesNotMatch hoặc Unable to locate credentials, chạy lại aws configure --profile nexusoms-demo và nhập đúng cặp key còn Active.

Ảnh cần chụp: 02-prerequisites/aws-cli/iam-user-and-profile.png. Chỉ chụp user name, policy name và kết quả aws sts; tuyệt đối không chụp Access Key, secret hoặc token.

**E.** Thu hồi quyền sau workshop

1\. IAM → Users → nexusoms-demo-deployer → Security credentials.

2\. Chọn Access Key đã tạo → Actions → Deactivate; kiểm tra không còn sử dụng rồi Delete.

3\. Trong Permissions, chọn AdministratorAccess → Remove.

4\. Xóa IAM user nếu không còn dùng cho lần triển khai khác.

5\. Trên máy local, chạy aws configure list-profiles. Chỉ xóa profile nexusoms-demo trong hai file AWS nếu còn profile khác.

## 2.3. Xác minh tài khoản AWS và lấy ID cần dùng

Chạy các lệnh này trong PowerShell để lấy những ID không cần đoán:

```
$Region = 'ap-southeast-1'
aws sts get-caller-identity
aws ec2 describe-availability-zones --region $Region --query 'AvailabilityZones[].[ZoneName,State]' --output table
aws ec2 describe-managed-prefix-lists --region $Region --filters Name=prefix-list-name,Values=com.amazonaws.global.cloudfront.origin-facing --query 'PrefixLists[0].[PrefixListId,PrefixListName]' --output table
```

**Kết quả cần thấy:** đúng AWS Account của bạn; các AZ đang available; prefix list có ID pl-... và tên com.amazonaws.global.cloudfront.origin-facing. Ghi lại prefix list ID để dùng làm nguồn inbound cho Security Group ALB.

Ảnh cần chụp: 02-prerequisites/aws-cli/aws-cli-and-validation.png. Che email, account ID nếu yêu cầu báo cáo; tuyệt đối không chụp Access Key, secret hoặc token.

## 3. Chuẩn bị máy local và source code

## 3.1. Kiểm tra phần mềm

Mở Windows PowerShell. Chạy từng lệnh:

```
git --version
node --version
npm --version
dotnet --version
docker version
aws --version
```

Docker phải hiện cả phần Client và Server. Nếu chỉ có Client hoặc báo không kết nối dockerDesktopLinuxEngine, mở Docker Desktop, chờ trạng thái Engine chạy rồi chạy lại docker version. Chưa có phần Server thì dừng, chưa build image.

## 3.2. Lấy nhánh mới nhất

```
Set-Location E:\Noxus-aws\nexus-aws
git status --short
git switch main
git pull --ff-only origin main
git rev-parse --short HEAD
```

Nếu git status --short liệt kê thay đổi mà bạn chưa commit, dừng và xử lý chúng trước; không dùng reset --hard. Ghi lại commit hash vừa in ra để biết image build từ source nào.

## 3.3. Chạy kiểm tra trước khi đẩy

```
Set-Location E:\Noxus-aws\nexus-aws\backend
dotnet test .\tests\NexusOMS.ApiTests\NexusOMS.ApiTests.csproj
docker build -t nexusoms-api:local-check .
```

Dockerfile backend hiện cài libgssapi-krb5-2 trong image runtime. Thư viện này giúp Npgsql nạp GSSAPI/Kerberos trên Linux; nếu image đang deploy báo Cannot load library libgssapi_krb5.so.2, task đang chạy image cũ hoặc Dockerfile mới chưa được build/push/deploy. Sau này luôn dùng tag image mới, không ghi đè tag bất biến.

Chạy test frontend từ PowerShell khác:

```
Set-Location E:\Noxus-aws\nexus-aws\frontend
npm ci
npm run typecheck
npm test
npm run build
```

Chỉ tiếp tục nếu các lệnh kết thúc thành công. Nếu lệnh trong package.json có tên khác trên nhánh mới, mở frontend/package.json và dùng script đã khai báo ở đó; không tự đoán.

## 3.4. Thông tin bí mật local

backend dùng .NET User Secrets cho chạy local. frontend/.env.local dùng biến Vite local và đã được .gitignore bỏ qua. Kiểm tra:

```
git check-ignore -v .\frontend\.env.local
```

Kết quả phải chỉ ra quy tắc .gitignore. Không đưa ConnectionStrings:Postgres, webhook secret, Supabase service-role key hoặc JWT vào .env frontend, commit, issue GitHub hay ảnh chụp.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**Lệnh PowerShell**

```text
git clone https://github.com/ThanhBinhDuongNguyen/nexus-aws.git
```

**Lệnh PowerShell**

```text
cd nexus-aws
```

**Region**

```text
ap-southeast-1
```

**Tài khoản và môi trường local - giá trị 4**

```text
nexusoms-demo-deployer
```

**Tài khoản và môi trường local - giá trị 5**

```text
nexusoms-demo-powershell
```

**C**

```text
aws configure --profile nexusoms-demo
```

**D**

```text
$env:AWS_PROFILE = 'nexusoms-demo'
```

**D**

```text
aws sts get-caller-identity --profile nexusoms-demo --region ap-southeast-1
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open IAM user dashboard

Trên AWS Console, thực hiện **Open IAM user dashboard** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/1-open-iam-user-dashboard.png" title="Open IAM user dashboard" >}}

### Bước 2 - Create IAM user button

Trên AWS Console, thực hiện **Create IAM user button** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/2-create-iam-user-button.png" title="Create IAM user button" >}}

### Bước 3 - Configure Specify user detail

Trên AWS Console, thực hiện **Configure Specify user detail** như hình dưới đây.

**Giá trị dùng ở bước này**

**Region**

```text
ap-southeast-1
```

**Tài khoản và môi trường local - giá trị 4**

```text
nexusoms-demo-deployer
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/3-configure-specify-user-detail.png" title="Configure Specify user detail" >}}

### Bước 4 - Configure permission

Trên AWS Console, thực hiện **Configure permission** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/4-configure-permission.png" title="Configure permission" >}}

### Bước 5 - Select Permissions Policies

Trên AWS Console, thực hiện **Select Permissions Policies** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/5-select-permissions-policies.png" title="Select Permissions Policies" >}}

### Bước 6 - Click Create User

Trên AWS Console, thực hiện **Click Create User** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/6-click-create-user.png" title="Click Create User" >}}

### Bước 7 - Complete create IAM User

Trên AWS Console, thực hiện **Complete create IAM User** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/7-complete-create-iam-user.png" title="Complete create IAM User" >}}

### Bước 8 - Configure Security credentials for user

Trên AWS Console, thực hiện **Configure Security credentials for user** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/8-configure-security-credentials-for-user.png" title="Configure Security credentials for user" >}}

### Bước 9 - Create access key

Trên AWS Console, thực hiện **Create access key** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/9-create-access-key.png" title="Create access key" >}}

### Bước 10 - Configure Command Link Interface + I understand... + Next

Trên AWS Console, thực hiện **Configure Command Link Interface + I understand... + Next** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/10-configure-command-link-interface-i-understand-next.png" title="Configure Command Link Interface + I understand... + Next" >}}

### Bước 11 - Set description tag

Trên AWS Console, thực hiện **Set description tag** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/11-set-description-tag.png" title="Set description tag" >}}

### Bước 12 - Copy access key + download csv

Trên AWS Console, thực hiện **Copy access key + download csv** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/12-copy-access-key-download-csv.png" title="Copy access key + download csv" >}}

### Bước 13 - Open PowerShell

Trên AWS Console, thực hiện **Open PowerShell** như hình dưới đây.

**Giá trị dùng ở bước này**

**Tài khoản và môi trường local - giá trị 5**

```text
nexusoms-demo-powershell
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/13-open-powershell.png" title="Open PowerShell" >}}

### Bước 14 - Check AWS version + Configure profile

Trên AWS Console, thực hiện **Check AWS version + Configure profile** như hình dưới đây.

**Giá trị dùng ở bước này**

**Lệnh PowerShell**

```text
git clone https://github.com/ThanhBinhDuongNguyen/nexus-aws.git
```

**Lệnh PowerShell**

```text
cd nexus-aws
```

**C**

```text
aws configure --profile nexusoms-demo
```

**D**

```text
$env:AWS_PROFILE = 'nexusoms-demo'
```

**D**

```text
aws sts get-caller-identity --profile nexusoms-demo --region ap-southeast-1
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/14-check-aws-version-configure-profile.png" title="Check AWS version + Configure profile" >}}

### Bước 15 - Configure more

Trên AWS Console, thực hiện **Configure more** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/15-configure-more.png" title="Configure more" >}}

### Bước 16 - Get available zone + CloudFront

Trên AWS Console, thực hiện **Get available zone + CloudFront** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/16-get-available-zone-cloudfront.png" title="Get available zone + CloudFront" >}}

### Bước 17 - Checking library in cmd

Trên AWS Console, thực hiện **Checking library in cmd** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/17-checking-library-in-cmd.png" title="Checking library in cmd" >}}

### Bước 18 - Run test for BE

Trên AWS Console, thực hiện **Run test for BE** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/18-1-run-test-for-be.png" title="Run test for BE" >}}

### Bước 19 - Docker build project

Trên AWS Console, thực hiện **Docker build project** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/18-2-docker-build-project.png" title="Docker build project" >}}

### Bước 20 - CI & type-check FE

Trên AWS Console, thực hiện **CI & type-check FE** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/19-1-ci-type-check-fe.png" title="CI & type-check FE" >}}

### Bước 21 - Run test + run build FE

Trên AWS Console, thực hiện **Run test + run build FE** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/iam-user-and-local-setup/19-2-run-test-run-build-fe.png" title="Run test + run build FE" >}}
