---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

Tại phần này, bạn cần tóm tắt nội dung workshop NexusOMS mà nhóm **dự tính** thực hiện và demo trên AWS.

# NexusOMS - Nền tảng Vận hành Đơn hàng và Tồn kho Đa kênh  
## Giải pháp AWS thực tiễn cho vận hành đơn hàng, tồn kho và lợi nhuận  

### 1. Tóm tắt điều hành  
NexusOMS là hệ thống cho một tổ chức kinh doanh qua Shopee, TikTok Shop, Lazada và Website D2C, tập trung dữ liệu sản phẩm, kho, tồn, đơn hàng sàn, ánh xạ SKU và lợi nhuận từng đơn. MVP hiện tại dùng giao diện React/Vite, API .NET 10 theo kiến trúc Modular Monolith và Supabase cho PostgreSQL cùng xác thực. Hệ thống có ba vai trò `admin`, `manager` và `warehouse`.

Trọng tâm của hệ thống là an toàn vận hành thay vì chỉ CRUD: giữ tồn nguyên tử, idempotency, webhook mô phỏng có chữ ký HMAC, retry worker, lịch sử biến động kho và lợi nhuận CM1–CM3. Nhóm chưa có gian hàng hay production credential của các sàn nên dùng simulator với payload lấy cảm hứng từ API marketplace để trình diễn toàn bộ vòng đời đơn.

### 2. Tuyên bố vấn đề  
*Vấn đề hiện tại*  
Người bán đa kênh thường phải chuyển qua nhiều dashboard để theo dõi đơn và tồn. Đơn flash sale xảy ra đồng thời có thể bán lố một SKU; webhook bị gửi lặp có thể tạo bản ghi trùng; còn lợi nhuận thực khó nhìn rõ sau giá vốn, phí thanh toán, vận hành và quảng cáo.

*Giải pháp*  
NexusOMS nhận webhook marketplace mô phỏng, kiểm tra chữ ký HMAC, ánh xạ SKU kênh về sản phẩm nội bộ và giữ tồn khả dụng trong PostgreSQL transaction. Đơn sau đó đi qua xác nhận, chuẩn bị, xuất kho/vận chuyển và giao thành công. Idempotency key cùng ràng buộc database ngăn việc webhook lặp tạo ra thao tác nghiệp vụ thứ hai. Giao diện cho phép theo dõi luồng vận hành, tồn kho, ánh xạ kênh và CM1–CM3 theo đơn/kênh.

*Lợi ích và hoàn vốn đầu tư (ROI)*  
Hệ thống tạo một góc nhìn vận hành thống nhất cho sản phẩm, tồn và đơn, giảm đối soát thủ công giữa các kênh. Luồng reserve, release và commit giúp minh họa flash sale có kiểm soát và có thể truy vết. Kiến trúc có thể triển khai AWS trong thời gian ngắn để chụp báo cáo/thuyết trình, sau đó xóa để tránh chi phí chạy liên tục. **PLACEHOLDER - bổ sung giả định kinh doanh, số giờ tiết kiệm và phép tính ROI của nhóm.**  

### 3. Kiến trúc giải pháp  
Ứng dụng hiện chạy local bằng React/Vite, .NET 10 API và Supabase Cloud. Kiến trúc mục tiêu cho demo đặt frontend trên Amazon S3 qua CloudFront; API đi qua Application Load Balancer và chạy bằng Amazon ECS Fargate trong hai private subnet thuộc hai Availability Zone. NAT Gateway cho phép task đi ra Supabase Cloud và endpoint marketplace mô phỏng. Hạ tầng gồm hai public subnet, hai private subnet, WAF và ACM ở lớp public edge.

![Kiến trúc luồng nghiệp vụ NexusOMS](/images/2-Proposal/Noxus_AWS_Architect_1.png)

*Dịch vụ AWS sử dụng*  
- *Amazon S3 và Amazon CloudFront*: Lưu trữ và phân phối React/Vite build tĩnh.  
- *AWS WAF và AWS Certificate Manager*: Bảo vệ traffic công khai và cung cấp chứng chỉ TLS.  
- *Application Load Balancer*: Định tuyến HTTPS API tới ECS task khỏe.  
- *Amazon ECS trên AWS Fargate*: Chạy container .NET 10 qua hai Availability Zone.  
- *Amazon ECR*: Lưu Docker image backend có phiên bản từ GitHub CI.  
- *Amazon VPC, NAT Gateway và Internet Gateway*: Cô lập API task trong private subnet nhưng vẫn cho phép outbound có kiểm soát.  
- *VPC Endpoints, AWS Secrets Manager và Amazon CloudWatch*: Truy xuất image/secret riêng tư khi phù hợp và thu log/metric.  
- *AWS CloudTrail và Amazon S3*: Ghi nhận AWS API activity và lưu access/load-balancer logs.  
- *Supabase Cloud*: Tiếp tục là PostgreSQL/Auth bên ngoài AWS; MVP này không thay bằng Amazon RDS.  

*Thiết kế thành phần*  
- *Frontend*: React/Vite hiển thị sản phẩm, kho, đơn hàng, lợi nhuận và webhook simulator.  
- *API*: .NET 10 Modular Monolith cung cấp REST API, xác thực Supabase JWT và áp dụng role policy.  
- *Luồng đơn/tồn*: PostgreSQL transaction giữ, nhả hoặc commit tồn và lưu inventory operation.  
- *Luồng webhook*: HMAC, idempotency và retry bảo vệ marketplace-style event mô phỏng.  
- *Database và identity*: Supabase cung cấp PostgreSQL/Auth; dữ liệu nằm trong `public` schema của một tổ chức.  
- *AWS runtime*: ECS task lấy secret an toàn, gửi log đến CloudWatch và đi ra Supabase/marketplace simulator qua NAT Gateway.  

### 4. Triển khai kỹ thuật  
*Các giai đoạn triển khai*  
Dự án gồm 2 phần - hoàn thiện MVP NexusOMS và chuẩn bị AWS demo - mỗi phần trải qua 4 giai đoạn:  
1. *Nghiên cứu và vẽ kiến trúc*: Phân tích nghiệp vụ đơn/tồn, rà soát codebase và vẽ kiến trúc AWS mục tiêu.  
2. *Tính toán chi phí và kiểm tra tính khả thi*: Sử dụng AWS Pricing Calculator để ước tính theo region và thời gian demo, sau đó điều chỉnh nếu cần.  
3. *Điều chỉnh kiến trúc để tối ưu chi phí/giải pháp*: Rà soát security group, IAM, NAT, image delivery và phạm vi cleanup trước khi deploy.  
4. *Phát triển, kiểm thử, triển khai*: Test frontend/backend local, build container API, deploy AWS services được chọn, xác nhận log và xóa tài nguyên tạm sau demo.  

*Yêu cầu kỹ thuật*  
- *Ứng dụng NexusOMS*: React/Vite, .NET 10 API, PostgreSQL/Npgsql, Supabase Auth JWT, role policy và HMAC webhook validation.  
- *AWS Demo*: Docker image trên ECR, ECS Fargate sau ALB, S3/CloudFront cho frontend, hai public và hai private subnet, NAT Gateway, CloudWatch logs, Secrets Manager cùng IAM role. Giá trị môi trường Supabase được lưu trong secret, không commit vào source code.  

### 5. Lộ trình & Mốc triển khai  
- *Chuẩn bị*: Chốt luồng source code, test case, sơ đồ kiến trúc và bảng tính AWS.  
- *Thiết lập demo*:  
    - Cấu hình network, IAM, ECR, S3/CloudFront và observability.  
    - Deploy container service, cấu hình ALB, TLS và secrets.  
    - Chạy demo webhook/đơn hàng/tồn kho, sau đó chụp bằng chứng cho báo cáo.  
- *Sau demo*: Xóa tài nguyên AWS tạm; chỉ giữ code, sơ đồ, screenshot và bằng chứng chi phí cho báo cáo.  

### 6. Ước tính ngân sách  
**PLACEHOLDER - chèn kết quả AWS Pricing Calculator theo region và thời lượng demo mà nhóm chọn.**  

Hoặc tải [tệp ước tính ngân sách](../attachments/budget_estimation.pdf).  

*Chi phí hạ tầng*  
- AWS Services:  
    - **PLACEHOLDER - chi phí CPU/memory cho Amazon ECS Fargate task.**  
    - **PLACEHOLDER - chi phí Application Load Balancer và load-balancer capacity.**  
    - **PLACEHOLDER - chi phí theo giờ/xử lý dữ liệu của NAT Gateway trong thời gian demo.**  
    - **PLACEHOLDER - ước tính S3, CloudFront, ECR, CloudWatch, Secrets Manager và data transfer.**  
    - **PLACEHOLDER - WAF, VPC endpoint, CloudTrail/log storage và dịch vụ tùy chọn được chọn.**  

*Tổng*: **PLACEHOLDER - điền tổng chi phí trong cửa sổ demo dự kiến.**  
- *Phần cứng*: Không cần mua thêm; dự án dùng máy phát triển hiện có và Supabase Cloud project.  

### 7. Đánh giá rủi ro  
*Ma trận rủi ro*  
- Chưa có marketplace credential: Ảnh hưởng trung bình, xác suất cao.  
- Xung đột tồn hoặc webhook trùng: Ảnh hưởng cao, xác suất trung bình.  
- Vượt ngân sách AWS trong demo: Ảnh hưởng trung bình, xác suất thấp.  

*Chiến lược giảm thiểu*  
- Marketplace: dùng simulator có HMAC và payload đại diện được ghi nhận.  
- Đơn/tồn: dùng idempotency key, ràng buộc database và transaction reserve/release/commit.  
- Chi phí: tạo AWS Budget/billing alert, giới hạn thời gian demo và xóa tài nguyên tính phí sau đó.  

*Kế hoạch dự phòng*  
- Chạy demo chức năng đầy đủ local với Supabase nếu dịch vụ AWS triển khai gặp sự cố.  
- Giữ lại định nghĩa hạ tầng và sơ đồ để có thể tái tạo, rồi dọn môi trường demo một cách có kiểm soát.  

### 8. Kết quả kỳ vọng  
*Cải tiến kỹ thuật*: Một hệ thống minh họa ánh xạ SKU, tồn kho, vòng đời đơn, an toàn webhook và lợi nhuận thay cho các thao tác thủ công rời rạc.  
*Giá trị dài hạn*: MVP cùng kiến trúc AWS tạo nền tảng có thể tái sử dụng khi có credential sàn thật, webhook adapter production và báo cáo nâng cao.  
