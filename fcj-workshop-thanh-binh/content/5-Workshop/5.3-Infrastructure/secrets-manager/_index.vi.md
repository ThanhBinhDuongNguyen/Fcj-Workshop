---
title: "Secrets Manager"
date: 2026-11-17
weight: 9
chapter: false
---

Lưu bốn giá trị trong một secret JSON. Không tạo bốn secret riêng.

## 1. Chuẩn bị giá trị

| JSON key | Giá trị cần dùng |
| --- | --- |
| `postgresConnectionString` | Chuỗi Npgsql của Session pooler, có database password |
| `supabaseUrl` | Project URL, không phải connection string |
| `systemActorId` | UUID của profile demo có `is_active=true` |
| `webhookSigningSecret` | Chuỗi ngẫu nhiên dài ít nhất 32 ký tự |

Lấy các giá trị từ Supabase:

- **Session pooler:** chọn **Connect → Session pooler** rồi sao chép host, port và username.
- **Database password:** dùng password của Supabase database.
- **Project URL:** mở **Project Settings → API → Project URL**.
- **Profile UUID:** mở **SQL Editor → New query** rồi chạy:

```sql
select id, email
from public.profiles
where lower(email) = lower('<EMAIL_DANG_NHAP>')
limit 1;
```

Sao chép UUID của đúng tài khoản demo. Không tự tạo UUID ngẫu nhiên.

## 2. Tạo secret

Mở **AWS Secrets Manager → Store a new secret**.

![Mở Secrets Manager và tạo secret](/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/1-open-secrets-manager-service-store-a-new-secret.png)

Chọn:

```text
Secret type: Other type of secret
```

Chuyển sang tab **Plaintext**. Thay toàn bộ placeholder rồi dán nguyên khối JSON sau:

```json
{
  "postgresConnectionString": "Host=<SESSION_POOLER_HOST>;Port=5432;Database=postgres;Username=<POOLER_USERNAME>;Password=<DATABASE_PASSWORD>;SSL Mode=Require;Trust Server Certificate=true",
  "supabaseUrl": "https://<PROJECT_REF>.supabase.co",
  "systemActorId": "<PROFILE_UUID>",
  "webhookSigningSecret": "<RANDOM_SECRET_AT_LEAST_32_CHARACTERS>"
}
```
![Chọn loại secret](/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/2-choose-secret-type.png)

Chọn **Next** và nhập:

```text
Secret name: nexusoms/demo/application
```

![Nhập JSON secret](/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/3-configure-secret.png)


Chọn **Next → Next → Store**.

## 3. Sao chép Secret ARN

Mở secret vừa tạo và sao chép **Secret ARN** đầy đủ.

![Sao chép Secret ARN](/images/5-Workshop/nexusoms/5.3-Infrastructure/secrets-manager/4-copy-secret-arn.png)

Lưu ARN để cấp quyền cho `nexusoms-demo-ecs-execution-role` ở bước tiếp theo.

## 4. Kiểm tra

Mở **Secret value → Retrieve secret value** và xác nhận có đúng bốn key:

```text
postgresConnectionString
supabaseUrl
systemActorId
webhookSigningSecret
```

Kiểm tra định dạng connection string mà không in password:

```powershell
$AwsProfile = "nexusoms-demo"
$AwsRegion = "ap-southeast-1"
$SecretId = "nexusoms/demo/application"

$Secret = aws secretsmanager get-secret-value --profile $AwsProfile --region $AwsRegion --secret-id $SecretId --query SecretString --output text | ConvertFrom-Json
$Builder = [System.Data.Common.DbConnectionStringBuilder]::new()
$ParsedActorId = [guid]::Empty

try {
  $Builder.set_ConnectionString([string]$Secret.postgresConnectionString)
  "postgresConnectionString: valid"
} catch {
  "postgresConnectionString: invalid"
}

"supabaseUrl: $([uri]::IsWellFormedUriString([string]$Secret.supabaseUrl, [System.UriKind]::Absolute))"
"systemActorId: $([guid]::TryParse([string]$Secret.systemActorId, [ref]$ParsedActorId))"
"webhookSigningSecret: $(([string]$Secret.webhookSigningSecret).Length -ge 32)"
```

Chỉ tiếp tục khi bốn dòng kiểm tra đều hợp lệ.

Kiểm tra trên chỉ xác nhận cấu trúc JSON, URL, UUID và cú pháp connection string. Bằng chứng kết nối database là task ECS mới khởi động thành công, `/health` trả `200` và target group chuyển sang `Healthy`.
