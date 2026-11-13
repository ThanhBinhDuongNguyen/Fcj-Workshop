---
title: "Tổng quan workshop"
date: 2026-11-13
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---

NexusOMS sử dụng CloudFront làm điểm truy cập duy nhất, S3 lưu frontend tĩnh, ALB chuyển tiếp API đến ECS Fargate trong private subnet, còn Supabase cung cấp PostgreSQL và xác thực.

## 1. Kiến trúc sẽ dựng

```
Trình duyệt
    │ HTTPS
    ▼
CloudFront distribution
    ├── Default behavior (*) ──► S3 riêng tư (frontend React/Vite)
    │                              └── CloudFront Function đưa /app?... về /index.html
    └── /api/* ─────────────────► ALB (HTTP:80 trong bản demo)
                                      │ chuyển tiếp tới target group (HTTP:8080)
                                      ▼
                                  ECS Fargate task trong private subnet
                                      └── NAT-A/NAT-B ──► ECR / Secrets Manager / CloudWatch Logs / Supabase
                                      └── S3 Gateway Endpoint gắn với hai private route table
```

VPC có hai public subnet để đặt ALB và hai NAT Gateway; hai private subnet tương ứng chạy ECS task. Private subnet A đi NAT-A, private subnet B đi NAT-B. ECR image pull, Secrets Manager, CloudWatch Logs, Supabase và API bên ngoài đi qua NAT. S3 dùng Gateway Endpoint gắn với hai private route table. ECS task không có public IP; S3 bucket frontend không public và CloudFront đọc file qua OAC.

Trình duyệt gọi API cùng origin CloudFront. Trong code, các endpoint đã có đường dẫn /api/v1/..., vì vậy VITE_API_BASE_URL là https://&lt;CLOUDFRONT_DOMAIN&gt; - không thêm /api, /api/v1, dấu / cuối, ALB DNS hay localhost.

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Noxus_AWS_Architect_1

Trên AWS Console, thực hiện **Noxus_AWS_Architect_1** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.1-Workshop-overview/Noxus_AWS_Architect_1.png" title="Noxus_AWS_Architect_1" >}}
