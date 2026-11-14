---
title: "Secrets Manager"
date: 2026-11-13
weight: 9
chapter: false
---

Lưu bốn giá trị trong một secret JSON. Không tạo bốn secret riêng và không đưa secret thật vào ảnh hoặc Git.

## 7.1. Tạo secret JSON

Mở Supabase Dashboard đúng project:

- **Project URL:** Project Settings → API → Project URL.
- **Database connection:** chọn **Connect → Session pooler**. Copy host, port và username Dashboard trả về; không tự ghép pooler host bằng region/project ref.
- **Database password:** dùng password mới đã đặt trong Supabase. Nếu password có ký tự đặc biệt, dùng chuỗi kết nối do Supabase cấp hoặc percent-encode theo hướng dẫn Supabase trước khi đưa vào URI; nếu nhập theo Npgsql key-value format, đặt password đúng dạng Npgsql và không làm lộ trong log.
- **Profile UUID:** làm theo truy vấn phần 7.3.

Sau đó AWS Console → **Secrets Manager → Store a new secret → Other type of secret**. Chọn nhập **Plaintext** JSON. Thay toàn bộ placeholder bằng giá trị thật, không giữ từ PENDING:

```
{
  "postgresConnectionString": "Host=<SESSION_POOLER_HOST>;Port=5432;Database=postgres;Username=<POOLER_USERNAME>;Password=<NEW_DATABASE_PASSWORD>;SSL Mode=Require;Trust Server Certificate=true",
  "supabaseUrl": "https://<PROJECT_REF>.supabase.co",
  "systemActorId": "<PROFILE_UUID>",
  "webhookSigningSecret": "<RANDOM_SECRET_AT_LEAST_32_CHARACTERS>"
}
```

Tên Secret: nexusoms/demo/application. Chọn Next, Next, Store. Secret có bốn JSON key trên; không tạo bốn secret riêng. Sau khi lưu, vào secret details và copy **Secret ARN** đầy đủ để gán quyền cho execution role.

## 7.2. Lấy systemActorId đúng tài khoản

Trong Supabase Dashboard → **SQL Editor → New query**, chạy câu sau, thay email bằng email đăng nhập của user dùng để demo:

```
select id, email
from public.profiles
where lower(email) = lower('<EMAIL_DANG_NHAP>')
limit 1;
```

Copy id dạng UUID của đúng dòng. Nếu không có kết quả, kiểm tra user đã có profile trong public.profiles chưa; không tự tạo UUID ngẫu nhiên. Giá trị này phải là profile hợp lệ, nếu không webhook simulator trả lỗi SystemActorId phải là UUID profile hợp lệ.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**Secrets Manager - giá trị 1**

```text
nexusoms/demo/application
```

**Plaintext**

```text
{
```

**Plaintext**

```text
"postgresConnectionString": "Host=<SESSION_POOLER_HOST>;Port=5432;Database=postgres;Username=<POOLER_USERNAME>;Password=<NEW_DATABASE_PASSWORD>;SSL Mode=Require;Trust Server Certificate=true",
```

**"supabaseUrl"**

```text
"supabaseUrl": "https://<PROJECT_REF>.supabase.co",
```

**"systemActorId"**

```text
"systemActorId": "<PROFILE_UUID>",
```

**"webhookSigningSecret"**

```text
"webhookSigningSecret": "<RANDOM_SECRET_AT_LEAST_32_CHARACTERS>"
```

**Secrets Manager - giá trị 7**

```text
}
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open Secrets Manager service + Store a new secret

Trên AWS Console, thực hiện **Open Secrets Manager service + Store a new secret** như hình dưới đây.

**Giá trị dùng ở bước này**

**Secrets Manager - giá trị 1**

```text
nexusoms/demo/application
```

**Plaintext**

```text
"postgresConnectionString": "Host=<SESSION_POOLER_HOST>;Port=5432;Database=postgres;Username=<POOLER_USERNAME>;Password=<NEW_DATABASE_PASSWORD>;SSL Mode=Require;Trust Server Certificate=true",
```

**Secrets Manager - giá trị 7**

```text
}
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/1-open-secrets-manager-service-store-a-new-secret.png" title="Open Secrets Manager service + Store a new secret" >}}

### Bước 2 - Choose secret type

Trên AWS Console, thực hiện **Choose secret type** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/2-choose-secret-type.png" title="Choose secret type" >}}

### Bước 3 - Configure secret

Trên AWS Console, thực hiện **Configure secret** như hình dưới đây.

**Giá trị dùng ở bước này**

**Plaintext**

```text
{
```

**"supabaseUrl"**

```text
"supabaseUrl": "https://<PROJECT_REF>.supabase.co",
```

**"systemActorId"**

```text
"systemActorId": "<PROFILE_UUID>",
```

**"webhookSigningSecret"**

```text
"webhookSigningSecret": "<RANDOM_SECRET_AT_LEAST_32_CHARACTERS>"
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/3-configure-secret.png" title="Configure secret" >}}

### Bước 4 - Copy secret ARN

Trên AWS Console, thực hiện **Copy secret ARN** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/4-copy-secret-arn.png" title="Copy secret ARN" >}}
