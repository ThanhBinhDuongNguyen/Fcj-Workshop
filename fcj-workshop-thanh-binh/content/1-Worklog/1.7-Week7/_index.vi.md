---
title: "Worklog Tuần 7"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Mục tiêu tuần 7:
 
* Chuẩn hóa kế hoạch NexusOMS và tách rõ hiện trạng MVP với kiến trúc backend đích.
* Chốt schema database chuẩn và cấu trúc project backend.
* Xây dựng hoàn chỉnh frontend MVP bằng React và prototype database trên Supabase.
* Chuẩn bị Backend completion guide (sprint, test, definition of done) để triển khai backend .NET vào tuần sau.
### Các công việc cần triển khai trong tuần này:
 
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                             |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------------------ |
| 1   | - Viết lại kế hoạch thống nhất (v4.0): <br>&emsp; + Tách rõ hiện trạng MVP (frontend, mock engine, Supabase migrations) và kiến trúc backend đích (.NET modular monolith, Docker/AWS) <br>&emsp; + Chốt phạm vi MVP: 14 màn hình runtime gom thành 12 nhóm nghiệp vụ <br>&emsp; + Xác định 4 role MVP (Owner, Operations, Finance, Viewer) và việc che dữ liệu theo role <br>&emsp; + Xác định 5 giai đoạn triển khai và luồng demo end-to-end mục tiêu                                                                                                                                                                      | 26/10/2026   | 26/10/2026      | Kế hoạch thống nhất NexusOMS (v4.0) <br> Kế hoạch thống nhất NexusOMS (v3.0, để đối chiếu) |
| 2   | - Viết Backend completion guide: <br>&emsp; + Ranh giới module: Identity and Tenancy, Inventory and Channels, Ordering and Fulfillment, Finance and Reporting, Integration and Async <br>&emsp; + Quy tắc API và transaction: tenant context xử lý ở server, kiểm tra membership và role, idempotency key, tồn kho chỉ thay đổi qua ledger, luồng webhook (verify HMAC → lưu delivery → enqueue → worker → retry/DLQ) <br>&emsp; + Inventory contract tối thiểu: resolve SKU, reserve, commit outbound, release, return inbound <br>&emsp; + 6 sprint (S1–S6) kèm tiêu chí nghiệm thu, chiến lược test và definition of done | 27/10/2026   | 27/10/2026      | Backend completion guide NexusOMS <br> Kế hoạch thống nhất NexusOMS (v4.0)                 |
| 3   | - Thiết kế database và cấu trúc project: <br>&emsp; + Viết lại `Database.dbml` thành schema chuẩn, đồng bộ với Supabase migrations (PostgreSQL 16, public schema, mọi bảng nghiệp vụ gắn tenant) <br>&emsp; + 24 bảng và 6 enum, gồm identity và tenancy, catalog và sổ cái tồn kho, kênh bán, đơn hàng, đổi trả, journal tài chính, webhook delivery log và audit log <br>&emsp; + Cấu trúc solution Modular Monolith: `NexusOMS.Api`, `NexusOMS.Contracts`, `Modules/InventoryAndChannels`, `Modules/OrderingAndFinance`                                                                                                   | 28/10/2026   | 28/10/2026      | `Database.dbml` <br> Tài liệu cấu trúc dự án <br> Supabase migrations                      |
| 4   | - Xây dựng frontend MVP (phần 1): <br>&emsp; + Ứng dụng React 18 + TypeScript + Vite, chạy local ở cổng 5174 <br>&emsp; + Điều hướng theo role và che dữ liệu theo role (Owner, Operations, Finance, Viewer) <br>&emsp; + Mock engine làm nguồn dữ liệu mặc định <br>&emsp; + Lớp data adapter để chuyển từng service từ mock sang Supabase/API                                                                                                                                                                                                                                                                              | 29/10/2026   | 29/10/2026      | Mã nguồn frontend NexusOMS <br> Kế hoạch thống nhất NexusOMS (v4.0)                        |
| 5   | - Xây dựng frontend MVP (phần 2): <br>&emsp; + 14 màn hình runtime trong 12 nhóm nghiệp vụ: dashboard, kho, đơn hàng, fulfillment, đổi trả, khách hàng, order economics, SKU economics, channel economics, cash and working capital, pricing và kết nối sàn <br>&emsp; + Nhóm kết nối sàn gồm 3 màn hình: kết nối, ánh xạ SKU và Flash Sale simulator <br>&emsp; + Chạy các luồng nghiệp vụ end-to-end qua mock engine                                                                                                                                                                                                       | 30/10/2026   | 30/10/2026      | Mã nguồn frontend NexusOMS <br> Kế hoạch thống nhất NexusOMS (v4.0)                        |
| 6   | - Dựng Supabase làm prototype cho tầng dữ liệu: <br>&emsp; + PostgreSQL trên Supabase theo migrations: schema multi-tenant với RLS/RPC <br>&emsp; + Giữ mock engine làm phương án dự phòng khi không có hạ tầng <br> - Tổng kết tuần và chuẩn bị chuyển sang backend .NET                                                                                                                                                                                                                                                                                                                                                    | 31/10/2026   | 31/10/2026      | Supabase project và migrations <br> `Database.dbml`                                        |
 
 
### Kết quả đạt được tuần 7:
 
* Hoàn thành một bản kế hoạch NexusOMS thống nhất (v4.0) trình bày đúng hiện trạng: đã có frontend React với mock engine và Supabase migrations; còn là kiến trúc đích: backend .NET, Docker, AWS. Nhờ vậy báo cáo không nói quá về những gì đã làm.
* Xác định phạm vi MVP và mô hình phân quyền:
  * 14 màn hình runtime gom thành 12 nhóm nghiệp vụ
  * 4 role với việc che dữ liệu riêng theo role (ví dụ Operations không xem được P&L và dòng tiền)
  * 5 giai đoạn triển khai: MVP local → data foundation → .NET API → async và storage → AWS deployment
* Viết Backend completion guide:
  * Ranh giới module và quyền sở hữu rõ ràng
  * Quy tắc phía server cho tenant context, phân quyền, idempotency và thay đổi tồn kho chỉ qua ledger
  * Luồng webhook an toàn: verify HMAC → lưu delivery → enqueue → worker → retry/DLQ
  * 6 sprint với tiêu chí nghiệm thu đo được (ví dụ: concurrent order test không được làm âm tồn kho, replay webhook không được tạo side effect lần hai)
  * Chiến lược test gồm unit, integration, API, E2E và load/concurrency
* Thay thiết kế cũ (14 bảng, 3 schema) bằng schema multi-tenant chuẩn gồm 24 bảng và 6 enum, đồng bộ với Supabase migrations để backend .NET kế thừa một schema duy nhất thay vì hai schema không tương thích.
* Chốt cấu trúc project Modular Monolith cho backend (`NexusOMS.Api`, `NexusOMS.Contracts` và các thư mục module).
* Hoàn thành frontend MVP (React 18, TypeScript, Vite) với điều hướng theo role, mock engine để demo local và lớp adapter cho phép chuyển từng service sang Supabase/API mà không phải viết lại giao diện.
* Dựng prototype Supabase cho tầng dữ liệu, sẵn sàng cho giai đoạn data foundation.
* Tuần sau: triển khai backend .NET theo completion guide, bắt đầu từ S1 (schema và auth, API skeleton).