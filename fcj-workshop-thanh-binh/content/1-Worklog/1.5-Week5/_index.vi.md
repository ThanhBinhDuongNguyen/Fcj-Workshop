---
title: "Worklog Tuần 5"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:
 
* Hoàn thành chuỗi lab cuối cùng trong lộ trình học FCJ (Document Management System — Serverless).
* Triển khai ứng dụng full-stack từ local lên AWS hoàn toàn bằng CLI.
* Bắt đầu giai đoạn Workshop project: học nền tảng C# và lên kế hoạch dự án NexusOMS.
### Các công việc cần triển khai trong tuần này:
 
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                                                                                                                                                                                                                             |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | - **Thực hành — Serverless Document Management System Series (Phần 1):** <br>&emsp; + Xây dựng CRUD Serverless với Lambda và DynamoDB <br>&emsp; + Lưu trữ và xác thực Serverless với AWS Amplify <br>&emsp; + Tích hợp Frontend với API Gateway <br>&emsp; + Triển khai hệ thống quản lý tài liệu với AWS SAM <br>&emsp; + Thiết lập phân phối nội dung với CloudFront <br>&emsp; + Thêm chức năng tìm kiếm <br>&emsp; + DevOps cho hệ thống quản lý tài liệu <br>&emsp; + Theo dõi phân tán với X-Ray và CloudWatch                                                                                                                                                                                                 | 12/10/2026   | 12/10/2026      | <https://000133.awsstudygroup.com/vi/> <br> <https://000134.awsstudygroup.com/vi/> <br> <https://000135.awsstudygroup.com/vi/> <br> <https://000136.awsstudygroup.com/vi/> <br> <https://000137.awsstudygroup.com/vi/> <br> <https://000138.awsstudygroup.com/vi/> <br> <https://000139.awsstudygroup.com/vi/> <br> <https://000140.awsstudygroup.com/vi/> |
| 2   | - **Thực hành — Serverless Document Management System Series (Phần 2 — Hoàn thành):** <br>&emsp; + Deploy full từ local lên AWS qua PowerShell và Ubuntu (WSL2) <br>&emsp; + Xử lý việc Cloud9 bị chặn bằng cách chạy toàn bộ lệnh từ máy local <br>&emsp; + Tự troubleshoot thư viện cũ: phát hiện và nâng cấp dependencies <br>&emsp; + Workshop Serverless API (000066) <br>&emsp; + CI/CD trên EKS với CodePipeline và GitHub (000062) — hoàn thành 90% <br> - 🏁 **Hoàn thành toàn bộ lộ trình lab FCJ**                                                                                                                                                                                                          | 13/10/2026   | 13/10/2026      | <https://000133.awsstudygroup.com/vi/> <br> <https://000066.awsstudygroup.com/vi/> <br> <https://000062.awsstudygroup.com/vi/> <br> PowerShell, Ubuntu (WSL2) <br> AWS CLI, AWS Amplify CLI, AWS SAM CLI                                                                                                                                                   |
| 3-5 | - Học nền tảng C# qua Coursera (Đại học FPT): <br>&emsp; + Class, interface, LINQ, async/await cơ bản <br>&emsp; + Nền tảng cần thiết trước khi bắt đầu dự án .NET <br> - Lên kế hoạch dự án Workshop OJT: **NexusOMS** <br>&emsp; + High-Concurrency Multi-Channel Order & Inventory Hub (.NET + PostgreSQL + AWS) <br>&emsp; + Phân tích thị trường: nghiên cứu các sản phẩm OMS hiện có (Sapo, Shopify, TikTok Shop) <br>&emsp; + Xác định 4 bài toán cốt lõi: chống bán lố, đồng bộ tồn kho đa kênh, idempotency, bảo mật PII <br>&emsp; + Lên kế hoạch 18 tuần với 3 giai đoạn, phân hạng Core / Stretch / Cắt <br>&emsp; + Xác định giới hạn kỹ thuật ngay từ đầu (không có Partner API thật, giới hạn FsCheck) | 14/10/2026   | 16/10/2026      | <https://www.coursera.org/organizations/fptuniversity/learn/c-sharp-programming-foundations> <br> Tài liệu NexusOMS Kế hoạch 18 tuần (bản tổng hợp, đã phản biện và hiệu chỉnh) <br> Sapo Omnichannel, Shopify Inventory API, TikTok Shop Partner Center Docs <br> Tham khảo: kgrzybek/modular-monolith-with-ddd, dotnet/eShop, Milan Jovanović            |
 
 
### Kết quả đạt được tuần 5:
 
* **Hoàn thành toàn bộ lộ trình lab FCJ Workforce** — cột mốc quan trọng sau 5 tuần thực hành liên tục với hơn 60 lab.
* Xây dựng hoàn thiện hệ thống quản lý tài liệu Serverless chạy hoàn toàn trên AWS:
  * Backend: Lambda + DynamoDB + S3 (CRUD đầy đủ)
  * Xác thực: AWS Amplify
  * Tầng API: API Gateway
  * Infrastructure as Code: AWS SAM (file YAML)
  * Phân phối nội dung: CloudFront
  * Tích hợp chức năng tìm kiếm
  * Theo dõi phân tán: X-Ray + CloudWatch
* Thể hiện kỹ năng CLI thành thạo khi deploy toàn bộ ứng dụng từ máy local lên AWS:
  * Tự điều chỉnh quy trình khi Cloud9 bị chặn — chuyển sang PowerShell + WSL2 mà không mất tiến độ
  * Tự xử lý toàn bộ lỗi: thư viện cũ cần nâng cấp, cấu hình IAM, lỗi API Gateway
  * Sử dụng CloudWatch logs hiệu quả để phân tích và debug lỗi Lambda
* Hoàn thành CI/CD pipeline trên EKS với CodePipeline và GitHub (90%) — bị giới hạn bởi Quota CodeBuild của tài khoản Free Tier.
* Chuyển sang giai đoạn Workshop project và hoàn thành kế hoạch ban đầu cho **NexusOMS**:
  * Phân tích thị trường các sản phẩm OMS hiện có để xác định phạm vi tính năng thực tế
  * Áp dụng nguyên tắc "sâu hơn rộng": ưu tiên Core để hiểu thấu đáo thay vì làm nông toàn bộ
  * Chọn kiến trúc Modular Monolith (ghi vào ADR) thay vì Microservice thuần túy — phù hợp với giai đoạn học tập và tránh tách service sai thời điểm
  * Xác định rõ giới hạn kỹ thuật ngay từ đầu (mock Partner API, giới hạn FsCheck) để trình bày trung thực khi phỏng vấn
  * Mỗi công nghệ trong stack phải trả lời được 4 câu hỏi: cơ chế hoạt động, khi nào thất bại, đánh đổi là gì, đo bằng số liệu nào
* Bắt đầu học nền tảng C# để chuẩn bị cho các phần kỹ thuật sâu trong dự án (.NET, Outbox, Kafka, Saga).


