---
title: "Kiểm tra source local"
date: 2026-11-20
weight: 4
chapter: false
---

Hoàn thành checkpoint local trước khi tạo tài nguyên AWS có tính phí.

## 1. Kiểm tra công cụ

Mở PowerShell tại thư mục gốc của repository:

```powershell
git --version
gh --version
aws --version
dotnet --version
node --version
npm --version
docker --version
docker info
```

Mở Docker Desktop và chuyển sang Linux containers nếu `docker info` chưa kết nối được Docker Engine.

## 2. Kiểm tra quyền GitHub

Nếu thực hiện phần CI/CD, kiểm tra quyền trên repository fork:

```powershell
gh auth status
gh repo view <GITHUB_OWNER>/nexus-aws --json nameWithOwner,viewerPermission
```

`viewerPermission` phải là `ADMIN` để mở repository Settings, tạo Actions secrets/variables và cấu hình OIDC. Tài khoản cũng phải có quyền tạo branch và pull request.

## 3. Kiểm tra backend

```powershell
dotnet restore backend/src/NexusOMS.Api/NexusOMS.Api.csproj
dotnet build backend/src/NexusOMS.Api/NexusOMS.Api.csproj -c Release
dotnet test backend/tests/NexusOMS.ApiTests/NexusOMS.ApiTests.csproj -c Release
```

## 4. Kiểm tra frontend

```powershell
Push-Location frontend
npm ci
npm run typecheck
npm run test
npm run build
Pop-Location
```

Chờ `npm ci` kết thúc trước khi chạy lệnh tiếp theo.

## 5. Kiểm tra Docker image

```powershell
docker build --file backend/Dockerfile --tag nexusoms-api:local-check backend
```

![Kiểm tra source và Docker image](/images/5-Workshop/nexusoms/5.2-Prerequisite/local-validation/1-run-tests-and-docker-build.png)

## 6. Cấu hình và chạy backend local

Mở PowerShell tại thư mục gốc của repository. Dùng thông tin đã lưu ở bước [Supabase project](../supabase-project/). Backend không tự đọc file `frontend/.env.local`.

Để lấy `PROFILE_UUID`, mở Supabase → **SQL Editor → New query** và chạy:

```sql
select id, email
from public.profiles
where lower(email) = lower('<EMAIL_DANG_NHAP>')
  and is_active = true
limit 1;
```

Dùng UUID của tài khoản demo có role `manager` hoặc `admin`. Thay các placeholder rồi chạy nguyên khối:

```powershell
$env:ConnectionStrings__Postgres = 'Host=<SESSION_POOLER_HOST>;Port=5432;Database=postgres;Username=<POOLER_USERNAME>;Password=<DATABASE_PASSWORD>;SSL Mode=Require;Trust Server Certificate=true'
$env:Supabase__Url = "https://<PROJECT_REF>.supabase.co"
$env:Frontend__Origin = "http://localhost:5173"
$env:WebhookSimulator__SystemActorId = "<PROFILE_UUID>"
$env:WebhookSimulator__Secret = ([guid]::NewGuid().ToString("N") + [guid]::NewGuid().ToString("N"))
$env:WebhookQueue__Mode = "Direct"

dotnet run --project backend/src/NexusOMS.Api/NexusOMS.Api.csproj --launch-profile http --urls "http://localhost:5145"
```

Giữ cửa sổ này chạy. Nếu mở PowerShell mới để chạy lại backend, đặt lại các biến trên. Chế độ `Direct` không cần SQS hay AWS credentials.

## 7. Chạy frontend và kiểm tra

Mở **cửa sổ PowerShell thứ hai** tại thư mục gốc của repository:

```powershell
Invoke-RestMethod -Method Get -Uri "http://localhost:5145/health"

Push-Location frontend
npm run dev -- --host localhost --port 5173 --strictPort
```

Lệnh `/health` trả `status: ok` chỉ xác nhận backend đang phản hồi.

Mở `http://localhost:5173`, đăng nhập bằng tài khoản demo rồi mở danh sách và chi tiết đơn hàng. Trong **DevTools → Network**, kiểm tra request `/api/v1/orders` tới `http://localhost:5145` trả `200` với dữ liệu JSON. Đây là bước kiểm tra backend đọc được database.

Mở **Webhook Simulator → Vòng đời đơn**, chọn channel connection, kho và SKU mapping từ dữ liệu seed rồi gửi một webhook tạo đơn. Chờ xử lý xong và mở chi tiết đơn vừa tạo.

Chỉ tiếp tục khi các bước build/test và kiểm tra local đều thành công. Nhấn `Ctrl+C` ở mỗi cửa sổ để dừng backend và frontend.
