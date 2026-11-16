---
title: "Supabase project"
date: 2026-11-15
weight: 2
chapter: false
---

## 1. Tạo project

Mở Supabase Dashboard → **New project**.

![Tạo Supabase project](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/1-open-dashboard-and-create-project.png)

Chọn organization, nhập tên project, database password và region gần Singapore. Lưu database password.

![Cấu hình database](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/2-configure-database.png)

## 2. Lấy thông tin kết nối

Mở **Project Settings → General** và sao chép Project ID.

![Sao chép Project ID](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/3-copy-project-id.png)

Chọn **Connect**.

![Mở thông tin kết nối](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/4-open-connect.png)

Chọn **Session pooler**, port `5432`.

![Chọn Session pooler](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/5-select-session-pooler.png)

Sao chép host và username để tạo connection string.

![Sao chép thông tin kết nối](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/6-copy-connection-values.png)

```text
Host=<SESSION_POOLER_HOST>;Port=5432;Database=postgres;Username=<POOLER_USERNAME>;Password=<DATABASE_PASSWORD>;SSL Mode=Require;Trust Server Certificate=true
```

Lưu connection string ở nơi an toàn.

![Lưu connection string](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/7-save-connection-string.png)

## 3. Chạy migration

```powershell
npx supabase@latest login
```

Mở liên kết đăng nhập do Supabase CLI cung cấp.

![Đăng nhập Supabase CLI](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/8-connect-supabase-cli.png)

Sao chép confirmation code vào terminal.

![Sao chép confirmation code](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/9-copy-confirmation-code.png)

![Supabase CLI đăng nhập thành công](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/10-cli-login-success.png)

Liên kết project và chạy migration:

```powershell
npx supabase@latest link --project-ref "<PROJECT_REF>"
npx supabase@latest db push --remote
```

Chờ lệnh hoàn tất.

![Đẩy migration lên Supabase](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/11-push-database-migrations.png)

Mở **Table Editor** và kiểm tra các bảng đã được tạo.

![Kiểm tra database tables](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/12-check-database-tables.png)

## 4. Tạo tài khoản demo

Mở **Authentication → Users → Add user**.

![Thêm user](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/13-open-authentication-and-add-user.png)

Tạo email và password dùng cho demo.

![Tạo tài khoản demo](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/14-create-demo-user.png)

Lấy UUID của user, mở bảng `public.profiles` và đặt role:

```text
manager
```

![Cập nhật role](/images/5-Workshop/nexusoms/5.2-Prerequisite/supabase-project/15-update-user-role.png)

Ghi lại các giá trị sau:

```text
PROJECT_REF=<PROJECT_REF>
SUPABASE_URL=https://<PROJECT_REF>.supabase.co
SUPABASE_ANON_KEY=<SUPABASE_PUBLISHABLE_OR_ANON_KEY>
PROFILE_UUID=<PROFILE_UUID>
POSTGRES_CONNECTION_STRING=<POSTGRES_CONNECTION_STRING>
```
