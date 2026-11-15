---
title: "Supabase project"
date: 2026-11-15
weight: 2
chapter: false
---

## 1. Tạo project

Mở Supabase Dashboard → **New project**.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/1-open-dashboard-and-create-project.png" title="Tạo Supabase project" >}}

Chọn organization, nhập tên project, database password và region gần Singapore. Lưu database password.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/2-configure-database.png" title="Cấu hình database" >}}

## 2. Lấy thông tin kết nối

Mở **Project Settings → General** và sao chép Project ID.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/3-copy-project-id.png" title="Sao chép Project ID" >}}

Chọn **Connect**.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/4-open-connect.png" title="Mở thông tin kết nối" >}}

Chọn **Session pooler**, port `5432`.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/5-select-session-pooler.png" title="Chọn Session pooler" >}}

Sao chép host và username để tạo connection string.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/6-copy-connection-values.png" title="Sao chép thông tin kết nối" >}}

```text
Host=<SESSION_POOLER_HOST>;Port=5432;Database=postgres;Username=<POOLER_USERNAME>;Password=<DATABASE_PASSWORD>;SSL Mode=Require;Trust Server Certificate=true
```

Lưu connection string ở nơi an toàn.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/7-save-connection-string.png" title="Lưu connection string" >}}

## 3. Chạy migration

```powershell
npx supabase@latest login
```

Mở liên kết đăng nhập do Supabase CLI cung cấp.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/8-connect-supabase-cli.png" title="Đăng nhập Supabase CLI" >}}

Sao chép confirmation code vào terminal.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/9-copy-confirmation-code.png" title="Sao chép confirmation code" >}}

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/10-cli-login-success.png" title="Supabase CLI đăng nhập thành công" >}}

Liên kết project và chạy migration:

```powershell
npx supabase@latest link --project-ref "<PROJECT_REF>"
npx supabase@latest db push --remote
```

Chờ lệnh hoàn tất.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/11-push-database-migrations.png" title="Đẩy migration lên Supabase" >}}

Mở **Table Editor** và kiểm tra các bảng đã được tạo.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/12-check-database-tables.png" title="Kiểm tra database tables" >}}

## 4. Tạo tài khoản demo

Mở **Authentication → Users → Add user**.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/13-open-authentication-and-add-user.png" title="Thêm user" >}}

Tạo email và password dùng cho demo.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/14-create-demo-user.png" title="Tạo tài khoản demo" >}}

Lấy UUID của user, mở bảng `public.profiles` và đặt role:

```text
manager
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/15-update-user-role.png" title="Cập nhật role" >}}

Ghi lại các giá trị sau:

```text
PROJECT_REF=<PROJECT_REF>
SUPABASE_URL=https://<PROJECT_REF>.supabase.co
SUPABASE_ANON_KEY=<SUPABASE_PUBLISHABLE_OR_ANON_KEY>
PROFILE_UUID=<PROFILE_UUID>
POSTGRES_CONNECTION_STRING=<POSTGRES_CONNECTION_STRING>
```
