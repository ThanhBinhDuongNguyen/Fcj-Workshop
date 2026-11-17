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

Trọng tâm của hệ thống là an toàn vận hành thay vì chỉ CRUD: giữ tồn nguyên tử, idempotency, webhook mô phỏng có chữ ký HMAC, hàng đợi SQS FIFO, lịch sử biến động kho và lợi nhuận CM1–CM3. Nhóm chưa có gian hàng hay production credential của các sàn nên dùng simulator với payload lấy cảm hứng từ API marketplace để trình diễn toàn bộ vòng đời đơn.

### 2. Tuyên bố vấn đề  
*Vấn đề hiện tại*  
Người bán đa kênh thường phải chuyển qua nhiều dashboard để theo dõi đơn và tồn. Đơn flash sale xảy ra đồng thời có thể bán lố một SKU; webhook bị gửi lặp có thể tạo bản ghi trùng; còn lợi nhuận thực khó nhìn rõ sau giá vốn, phí thanh toán, vận hành và quảng cáo.

*Giải pháp*  
NexusOMS nhận webhook marketplace mô phỏng, kiểm tra chữ ký HMAC rồi đưa sự kiện hợp lệ vào Amazon SQS FIFO. Worker trong ECS lấy message, ánh xạ SKU kênh về sản phẩm nội bộ và giữ tồn khả dụng trong PostgreSQL transaction. Đơn sau đó đi qua xác nhận, chuẩn bị, xuất kho/vận chuyển, giao thành công hoặc hủy. Khi hủy trước lúc xuất kho, hệ thống nhả phần tồn đang giữ; idempotency key cùng ràng buộc database ngăn webhook lặp tạo ra thao tác nghiệp vụ thứ hai. Message lỗi quá số lần cho phép được chuyển sang dead-letter queue để kiểm tra riêng.

*Lợi ích và hoàn vốn đầu tư (ROI)*  
Hệ thống tạo một góc nhìn vận hành thống nhất cho sản phẩm, tồn và đơn, giảm đối soát thủ công giữa các kênh. Luồng reserve, release và commit giúp minh họa flash sale có kiểm soát và có thể truy vết. Workshop tập trung chứng minh giải pháp kỹ thuật nên chưa quy đổi ROI tài chính khi chưa có dữ liệu vận hành thực tế.

### 3. Kiến trúc giải pháp  
Ứng dụng hiện chạy local bằng React/Vite, .NET 10 API và Supabase Cloud. Kiến trúc mục tiêu cho demo đặt frontend trên Amazon S3 qua CloudFront; API đi qua Application Load Balancer và chạy bằng Amazon ECS Fargate trong hai private subnet thuộc hai Availability Zone. Amazon SQS FIFO tách bước tiếp nhận webhook khỏi bước xử lý nghiệp vụ; dead-letter queue giữ các message thất bại nhiều lần. Mỗi Availability Zone có một NAT Gateway để task vẫn có đường outbound khi một AZ gặp sự cố. Cấu hình này tăng chi phí nhưng tránh phụ thuộc vào một NAT duy nhất; môi trường demo tiết kiệm có thể dùng một NAT và chấp nhận single point of failure. AWS WAF được thể hiện trong kiến trúc mục tiêu và dự toán nhưng không bật trong workshop. Workshop dùng domain cùng chứng chỉ mặc định của CloudFront nên không tạo AWS Certificate Manager. CloudTrail và ALB access logs được lưu trong một S3 audit bucket riêng tư.

![Kiến trúc luồng nghiệp vụ NexusOMS](/images/2-Proposal/noxus-aws-architect-1.png)

*Luồng hoạt động trên sơ đồ*

| Số | Luồng |
|---:|---|
| 1 | Người dùng truy cập hệ thống qua CloudFront. |
| 2 | CloudFront lấy giao diện React từ S3. |
| 3 | CloudFront chuyển request `/api/*` đến Application Load Balancer. |
| 4 | ALB phân phối request đến Fargate task đang healthy. |
| 5 | API xác thực webhook rồi gửi message vào SQS FIFO. |
| 6 | Worker trong Fargate nhận và xử lý message theo thứ tự của message group. |
| 7 | Message lỗi quá số lần nhận được chuyển sang dead-letter queue. |
| 8 | Fargate đi ra ngoài qua NAT Gateway của Availability Zone tương ứng. |
| 9 | NAT Gateway đi qua Internet Gateway. |
| 10 | Backend kết nối Supabase và các nền tảng bán hàng bên ngoài. |
| 11 | GitHub Actions build và đẩy backend image lên ECR. |
| 12 | GitHub Actions đăng ký task definition và cập nhật ECS service. |
| 13 | ECS triển khai image mới thành Fargate task ở hai Availability Zone. |
| 14 | Fargate truy cập S3 qua S3 Gateway Endpoint. |
| 15 | ALB ghi access log vào S3 audit bucket. |
| 16 | CloudTrail ghi management event vào S3 audit bucket. |

*Dịch vụ AWS sử dụng*  
- *Amazon S3 và Amazon CloudFront*: Lưu trữ và phân phối React/Vite build tĩnh.  
- *AWS WAF*: Thể hiện lớp bảo vệ ứng dụng mục tiêu và được tính trong dự toán, nhưng không bật trong workshop.  
- *Application Load Balancer*: Định tuyến API tới ECS task khỏe.  
- *Amazon ECS trên AWS Fargate*: Chạy container .NET 10 qua hai Availability Zone.  
- *Amazon ECR*: Lưu Docker image backend có phiên bản từ GitHub CI.  
- *Amazon SQS FIFO và Dead-letter Queue*: Đệm webhook tăng đột biến, giữ thứ tự theo nhóm và cô lập message xử lý lỗi.
- *Amazon VPC, NAT Gateway và Internet Gateway*: Cô lập API task trong private subnet nhưng vẫn cho phép outbound có kiểm soát.  
- *S3 Gateway Endpoint, AWS Secrets Manager và Amazon CloudWatch*: Định tuyến S3 riêng tư, quản lý secret và thu log/metric; ECR, Secrets Manager và CloudWatch Logs được truy cập qua NAT Gateway.  
- *AWS CloudTrail và Amazon S3*: Ghi management events và lưu CloudTrail/ALB access logs trong audit bucket riêng tư.  
- *Supabase Cloud*: Tiếp tục là PostgreSQL/Auth bên ngoài AWS vì ứng dụng đã dùng PostgreSQL và Supabase Auth, giúp bài lab tập trung vào triển khai AWS thay vì di chuyển database. Bản production có thể chuyển sang Amazon RDS hoặc Aurora PostgreSQL khi cần vận hành toàn bộ dữ liệu trong AWS.

*Thiết kế thành phần*  
- *Frontend*: React/Vite hiển thị sản phẩm, kho, đơn hàng, lợi nhuận và webhook simulator.  
- *API*: .NET 10 Modular Monolith cung cấp REST API, xác thực Supabase JWT và áp dụng role policy.  
- *Luồng đơn/tồn*: PostgreSQL transaction giữ, nhả hoặc commit tồn và lưu inventory operation.  
- *Luồng webhook bất đồng bộ*: API xác minh HMAC và gửi sự kiện vào SQS FIFO; worker ECS xử lý message, còn DLQ giữ message thất bại quá ba lần.
- *Luồng hủy đơn*: Sự kiện `order.cancelled` cập nhật trạng thái, lưu lý do hủy và nhả tồn đang giữ nếu đơn chưa xuất kho. Idempotency ngăn việc hoàn tồn hai lần.
- *Database và identity*: Supabase cung cấp PostgreSQL/Auth; dữ liệu nằm trong `public` schema của một tổ chức.  
- *AWS runtime*: ECS task lấy secret an toàn, gửi log đến CloudWatch, đọc/ghi SQS bằng task role và đi ra Supabase/marketplace simulator qua NAT Gateway.
- *CI/CD*: GitHub Actions dùng OIDC để nhận AWS credential ngắn hạn, đẩy image lên ECR, cập nhật ECS service, đồng bộ frontend lên S3 và tạo CloudFront invalidation.

### 4. Triển khai kỹ thuật  
*Các giai đoạn triển khai*  
Dự án gồm 2 phần - hoàn thiện MVP NexusOMS và chuẩn bị AWS demo - mỗi phần trải qua 4 giai đoạn:  
1. *Nghiên cứu và vẽ kiến trúc*: Phân tích nghiệp vụ đơn/tồn, rà soát codebase và vẽ kiến trúc AWS mục tiêu.  
2. *Tính toán chi phí và kiểm tra tính khả thi*: Sử dụng AWS Pricing Calculator để ước tính theo region và thời gian demo, sau đó điều chỉnh nếu cần.  
3. *Điều chỉnh kiến trúc để tối ưu chi phí/giải pháp*: Rà soát security group, IAM, NAT, image delivery và phạm vi cleanup trước khi deploy.  
4. *Phát triển, kiểm thử, triển khai*: Test frontend/backend local, build container API, deploy AWS services được chọn, xác nhận log và xóa tài nguyên tạm sau demo.  

*Yêu cầu kỹ thuật*  
- *Ứng dụng NexusOMS*: React/Vite, .NET 10 API, PostgreSQL/Npgsql, Supabase Auth JWT, role policy và HMAC webhook validation.  
- *AWS Demo*: Docker image trên ECR, ECS Fargate sau ALB, SQS FIFO và DLQ cho webhook, S3/CloudFront cho frontend, hai public và hai private subnet, NAT Gateway, CloudWatch logs, Secrets Manager cùng IAM role. Giá trị môi trường Supabase được lưu trong secret, không commit vào source code.

### 5. Lộ trình & Mốc triển khai  
- *Chuẩn bị*: Chốt luồng source code, test case, sơ đồ kiến trúc và bảng tính AWS.  
- *Thiết lập demo*:  
    - Cấu hình network, IAM, ECR, S3/CloudFront và observability.  
    - Deploy container service, cấu hình ALB, CloudFront, audit logs và secrets.  
    - Chạy demo webhook/đơn hàng/tồn kho và tải cao qua SQS, kiểm tra DLQ, sau đó dọn dữ liệu `DEMO-*` và chụp bằng chứng cho báo cáo.
- *Sau demo*: Xóa tài nguyên AWS tạm; chỉ giữ code, sơ đồ, screenshot và bằng chứng chi phí cho báo cáo.  

### 6. Ước tính ngân sách  
Dự toán được lập bằng AWS Pricing Calculator cho Region **Asia Pacific (Singapore) / ap-southeast-1**. Các mức dưới đây là chi phí tháng theo cấu hình và lượng sử dụng demo đã nhập vào Calculator; hóa đơn thực tế cho 10 ngày có thể khác do thời gian chạy từng tài nguyên, lưu lượng thực tế, Free Tier và thuế.

*Chi phí hạ tầng*  
| Dịch vụ | Ước tính (USD/tháng) |
| --- | ---: |
| Amazon ECS Fargate | 3.75 |
| Application Load Balancer | 18.98 |
| 2 NAT Gateway | 86.14 |
| Amazon S3 | 0.02 |
| Amazon ECR | 0.20 |
| Amazon CloudWatch | 0.07 |
| AWS Secrets Manager | 0.13 |
| Amazon CloudTrail | 0.00 |
| Amazon CloudFront | 0.00 |
| Amazon SQS FIFO và Dead-letter Queue | 0.05 |
| AWS WAF (ước tính riêng, không bật trong workshop nhưng vẫn tính vào ngân sách) | 14.00 |
| **Tổng có tính WAF** | **123.34** |

Chi phí SQS giả định `100.000` request FIFO có tính phí mỗi tháng với đơn giá `0,50 USD/triệu request`, chưa trừ Free Tier. Calculator cho kết quả **$0.00 chi phí trả trước**. WAF được cộng vào tổng ngân sách để thể hiện phương án có bảo vệ lớp ứng dụng, dù tài nguyên WAF không được bật trong workshop. Chi phí NAT Gateway và ALB chủ yếu phát sinh theo giờ; vì vậy không nên xem tổng theo tháng là hóa đơn chính xác cho 10 ngày. Với giả định phân bổ tuyến tính 10/30 ngày, tổng tham khảo gồm WAF là khoảng **$41.11** (không gồm thuế); số thực trả cần đối chiếu thời điểm tạo/xóa tài nguyên và mức sử dụng trong AWS Billing.

- *Phần cứng*: Không cần mua thêm; dự án dùng máy phát triển hiện có và Supabase Cloud project.  

### 7. Đánh giá rủi ro  
*Ma trận rủi ro*  
- Chưa có marketplace credential: Ảnh hưởng trung bình, xác suất cao.  
- Xung đột tồn hoặc webhook trùng: Ảnh hưởng cao, xác suất trung bình.  
- Vượt ngân sách AWS trong demo: Ảnh hưởng trung bình, xác suất thấp.  

*Chiến lược giảm thiểu*  
- Marketplace: dùng simulator có HMAC và payload đại diện được ghi nhận.  
- Đơn/tồn: dùng SQS FIFO, DLQ, idempotency key, ràng buộc database và transaction reserve/release/commit.
- Chi phí: tạo AWS Budget/billing alert, giới hạn thời gian demo và xóa tài nguyên tính phí sau đó.  

*Kế hoạch dự phòng*  
- Chạy demo chức năng đầy đủ local với Supabase nếu dịch vụ AWS triển khai gặp sự cố.  
- Giữ lại định nghĩa hạ tầng và sơ đồ để có thể tái tạo, rồi dọn môi trường demo một cách có kiểm soát.  

### 8. Kết quả kỳ vọng  
*Cải tiến kỹ thuật*: Một hệ thống minh họa ánh xạ SKU, tồn kho, vòng đời đơn gồm cả hủy đơn, xử lý webhook bất đồng bộ và lợi nhuận thay cho các thao tác thủ công rời rạc.
*Giá trị dài hạn*: MVP cùng kiến trúc AWS tạo nền tảng có thể tái sử dụng khi có credential sàn thật, webhook adapter production và báo cáo nâng cao.  
