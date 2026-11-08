---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Mục tiêu tuần 8:
 
* Thu gọn phạm vi NexusOMS theo đúng nhu cầu MVP và chuyển từ mô hình multi-tenant sang mô hình một tổ chức.
* Xây dựng và kiểm thử backend .NET trên Supabase (Auth + PostgreSQL) cho các luồng sản phẩm, kho, tồn kho và đơn hàng cốt lõi.
* Kết nối frontend với API thật và chuẩn bị luồng demo end-to-end (webhook simulator, flash sale, báo cáo lợi nhuận).
* Rà soát kiến trúc AWS mục tiêu và cập nhật tài liệu proposal.
### Các công việc cần triển khai trong tuần này:
 
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                    | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                       |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | -------------------------------------------------------------------- |
| 1   | - Rà soát project cũ, cắt các chức năng và trang không phục vụ MVP <br> - Thống nhất giữ Supabase và chuyển từ multi-tenant sang một tổ chức                                                                                                                                                                                                                                 | 01/11/2026   | 01/11/2026      | Repository NexusOMS                                                  |
| 2   | - Hoàn thiện nền tảng backend cho một tổ chức <br>&emsp; + Xây dựng CRUD Brand <br>&emsp; + Thiết lập cấu trúc backend .NET, Docker và CI/CD cơ bản                                                                                                                                                                                                                          | 02/11/2026   | 02/11/2026      | Repository NexusOMS                                                  |
| 3   | - Thiết lập Supabase project mới: tạo migration baseline, cấu hình `.env.local`, liên kết project Supabase và kiểm tra Git ignore để không lộ secret <br> - Xây dựng API Product, Warehouse và Inventory (bins/channels) <br> - Tạo luồng kiểm thử bằng Postman                                                                                                              | 03/11/2026   | 03/11/2026      | Repository NexusOMS <br> Supabase project và migrations <br> Postman |
| 4   | - Hoàn thiện luồng đơn hàng–tồn kho: tạo đơn, giữ tồn, xuất kho, hủy đơn và giao hàng <br> - Bổ sung transaction, idempotency, migration và kiểm thử                                                                                                                                                                                                                         | 04/11/2026   | 04/11/2026      | Repository NexusOMS <br> Postman                                     |
| 5   | - Kết nối frontend với API thật <br>&emsp; + Hoàn thiện khách hàng, kết nối sàn, P&L CM1–CM3 và dashboard <br>&emsp; + Xử lý lỗi token/auth <br> - Bắt đầu webhook simulator và dọn mock legacy                                                                                                                                                                              | 05/11/2026   | 05/11/2026      | Repository NexusOMS                                                  |
| 6   | - Hoàn thiện trải nghiệm demo: webhook theo từng giai đoạn, retry/audit tồn kho, mô phỏng flash sale, SKU mapping, biểu đồ lợi nhuận và kênh, tính giá bán <br> - Tối ưu các màn hình vận hành                                                                                                                                                                               | 06/11/2026   | 06/11/2026      | Repository NexusOMS                                                  |
| 7   | - Rà soát UI cuối: searchable select trong popup/modal không còn tràn màn hình <br> - Chỉnh layout bảng, bộ lọc kho, khách hàng, đơn hàng, tồn kho và pricing <br> - Kiểm tra build, typecheck và test frontend                                                                                                                                                              | 07/11/2026   | 07/11/2026      | Repository NexusOMS                                                  |
| 1-7 | - Rà soát kiến trúc AWS mục tiêu: CloudFront, S3, WAF, ACM, ALB, ECS Fargate hai AZ, hai public subnet và hai private subnet, NAT Gateway, ECR, Secrets Manager, CloudWatch, CloudTrail, VPC endpoint và Supabase Cloud <br> - Soạn và cập nhật proposal tiếng Anh và tiếng Việt, giữ khuôn workshop và chừa placeholder cho diagram, AWS Pricing Calculator, chi phí và ROI | 01/11/2026   | 07/11/2026      | Proposal NexusOMS                                                    |
 
 
### Kết quả đạt được tuần 8:
 
* Thu gọn phạm vi và thay đổi mô hình truy cập:
  * Cắt các chức năng và trang không phục vụ MVP
  * Chuyển từ multi-tenant sang một tổ chức với 3 role: admin, manager và warehouse
  * Giữ Supabase cho Auth và PostgreSQL
* Xây dựng nền tảng backend:
  * Tạo Supabase project mới với migration baseline và xử lý migration bị trùng cột
  * Kết nối backend .NET với Supabase PostgreSQL và Supabase JWT
  * Thiết lập cấu trúc backend, Docker và CI/CD cơ bản, kiểm tra secret không bị đưa lên Git
  * Hoàn thiện API Brand, Product, Warehouse và Inventory (bins/channels)
* Kiểm thử API bằng Postman: đăng nhập và lấy access token, Brand, Product, Warehouse, Inventory, Orders, Channel connection, SKU mapping và webhook.
* Hoàn thiện nghiệp vụ tồn kho: nhập kho, điều chỉnh, reserve, release và commit; ngăn tồn âm hoặc giảm thấp hơn lượng đang giữ.
* Hoàn thiện nghiệp vụ đơn hàng: tạo đơn cho khách vãng lai hoặc khách có sẵn, lịch sử trạng thái, hủy đơn, xuất kho, giao hàng và lợi nhuận đơn; được hỗ trợ bởi transaction, idempotency, migration và kiểm thử.
* Xây dựng webhook simulator cho demo sàn theo từng giai đoạn: tạo đơn → giữ tồn → xác nhận → chuẩn bị → xuất kho/vận chuyển → giao thành công, kèm retry/audit tồn kho và mô phỏng flash sale.
* Thiết kế lại frontend theo nghiệp vụ thực tế và kết nối với API thật:
  * Đơn sàn chỉ nhận trạng thái từ webhook
  * Product giữ đầy đủ CRUD, khách hàng được tạo từ đơn hàng
  * Thao tác tồn kho được đưa vào popup ngắn gọn
  * Chuẩn hóa việc gọi API vào thư mục service, xử lý thống nhất state, selection, search và filter
  * Biểu đồ Recharts cho Tổng quan, Lợi nhuận đơn và Hiệu quả kênh
  * Thiết kế lại trang đăng nhập và trang chủ, logo và favicon, popup và dropdown tìm kiếm
  * Chạy build, typecheck và test frontend trong lần rà soát UI cuối
* Rà soát kiến trúc AWS mục tiêu và cập nhật tài liệu proposal tiếng Anh và tiếng Việt, giữ placeholder cho diagram, AWS Pricing Calculator, chi phí và ROI.
* Kết quả: MVP đã có luồng end-to-end để demo từ sản phẩm, kho và ánh xạ SKU đến webhook đơn hàng, giữ tồn, xuất kho và báo cáo lợi nhuận. Phần AWS hiện là kiến trúc triển khai mục tiêu, chưa phải hạ tầng production chạy liên tục.