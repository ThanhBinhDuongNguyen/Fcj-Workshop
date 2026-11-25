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
Ứng dụng hiện chạy local bằng React/Vite, .NET 10 API và Supabase Cloud. Kiến trúc mục tiêu cho demo đặt frontend trên Amazon S3 qua CloudFront; API đi qua Application Load Balancer và chạy bằng Amazon ECS Fargate trong hai private subnet thuộc hai Availability Zone. Amazon SQS FIFO tách bước tiếp nhận webhook khỏi bước xử lý nghiệp vụ; dead-letter queue giữ các message thất bại nhiều lần. Mỗi Availability Zone có một NAT Gateway để task vẫn có đường outbound khi một AZ gặp sự cố. AWS WAF bảo vệ ứng dụng trước các request độc hại phổ biến. Workshop dùng domain cùng chứng chỉ mặc định của CloudFront nên không tạo AWS Certificate Manager. CloudTrail và ALB access logs được lưu trong một S3 audit bucket riêng tư.

![Kiến trúc luồng nghiệp vụ NexusOMS](/images/2-Proposal/noxus-aws-architect-1.png)

*Luồng hoạt động trên sơ đồ*

| Số | Luồng |
|---:|---|
| 1 | Người dùng truy cập hệ thống qua CloudFront. |
| 2 | CloudFront lấy giao diện React từ S3. |
| 3 | CloudFront chuyển request `/api/*` đến Application Load Balancer. |
| 4 | ALB phân phối request đến Fargate task đang healthy. |
| 5 | API xác thực webhook rồi gửi message vào SQS FIFO. |
| 6 | Background worker trong Fargate chủ động poll SQS, xử lý message theo thứ tự của message group và xóa message sau khi thành công. |
| 7 | Message lỗi quá số lần nhận được chuyển sang dead-letter queue. |
| 8 | Fargate đi ra ngoài qua NAT Gateway của Availability Zone tương ứng. |
| 9 | NAT Gateway đi qua Internet Gateway. |
| 10 | Backend kết nối Supabase; nhánh Open API tới nền tảng bán hàng thể hiện tích hợp mục tiêu, chưa triển khai trong demo. |
| 11 | GitHub Actions build và đẩy backend image lên ECR. |
| 12 | GitHub Actions đăng ký task definition và cập nhật ECS service. |
| 13 | ECS triển khai image mới thành Fargate task ở hai Availability Zone. |
| 14 | Khi khởi động task, Fargate tải image layers của ECR từ S3 do AWS quản lý qua S3 Gateway Endpoint. Số 14 áp dụng cho toàn tuyến Fargate → Endpoint → S3. |
| 15 | ALB ghi access log vào S3 audit bucket. |
| 16 | CloudTrail ghi management event vào S3 audit bucket. |

Đường nét đứt GitHub Actions → AWS STS thể hiện bước xác thực OIDC của CI/CD nên không đánh số trong luồng nghiệp vụ.

Hai Fargate trên sơ đồ là hai bản sao của cùng ứng dụng trong một ECS service. Mỗi task chạy API và background worker chung trong một tiến trình .NET. Luồng 5–6 áp dụng cho cả hai task, không phải giao tiếp giữa hai task hay hai service riêng biệt. Để trình diễn cấu hình hai AZ, đặt `Desired tasks = 2` và kiểm tra mỗi AZ có một task đang chạy; bài lab mặc định dùng một task.

Khối Open API của các nền tảng bán hàng là kiến trúc mục tiêu. Demo hiện dùng Webhook Simulator tích hợp trong ứng dụng để chủ động tạo sự kiện đặt đơn, hủy đơn và tải cao. Các thay đổi đơn hàng và tồn kho được kiểm chứng trong NexusOMS; chưa đồng bộ với gian hàng thật. Đường outbound ở luồng 10 không biểu diễn webhook từ sàn gửi vào hệ thống.

*Dịch vụ AWS sử dụng*  
- *Amazon S3 và Amazon CloudFront*: Lưu trữ và phân phối React/Vite build tĩnh.  
- *AWS WAF*: Bảo vệ ứng dụng trước các request độc hại phổ biến.
- *Application Load Balancer*: Định tuyến API tới ECS task khỏe.  
- *Amazon ECS trên AWS Fargate*: Chạy container .NET 10 qua hai Availability Zone.  
- *Amazon ECR*: Lưu Docker image backend có phiên bản từ GitHub CI.  
- *AWS STS*: Xác thực GitHub OIDC token và cấp credential tạm thời cho GitHub Actions.
- *Amazon SQS FIFO và Dead-letter Queue*: Đệm webhook tăng đột biến, giữ thứ tự theo nhóm và cô lập message xử lý lỗi.
- *Amazon VPC, NAT Gateway và Internet Gateway*: Cô lập API task trong private subnet nhưng vẫn cho phép outbound có kiểm soát.  
- *S3 Gateway Endpoint*: Tải image layers từ S3 do ECR sử dụng qua mạng riêng. Bucket này do AWS quản lý, khác với bucket frontend và audit. Backend hiện chưa lưu ảnh sản phẩm, hóa đơn hay báo cáo lên S3.
- *AWS Secrets Manager và Amazon CloudWatch*: Quản lý cấu hình ứng dụng và thu log/metric. ECR API/registry, Secrets Manager, CloudWatch Logs và SQS dùng NAT Gateway trong cấu hình lab; lưu lượng S3 dùng Gateway Endpoint.
- *AWS CloudTrail và Amazon S3*: Ghi management events và lưu CloudTrail/ALB access logs trong audit bucket riêng tư.  
- *Supabase Cloud*: Cung cấp cả PostgreSQL và Supabase Auth đã được ứng dụng tích hợp. Giữ Supabase giúp bài lab tập trung vào triển khai AWS. RDS PostgreSQL có thể thay phần database, nhưng cần xử lý riêng phần xác thực nếu thay toàn bộ Supabase.

*Thiết kế thành phần*  
- *Frontend*: React/Vite hiển thị sản phẩm, kho, đơn hàng, lợi nhuận và webhook simulator.  
- *API*: .NET 10 Modular Monolith cung cấp REST API, xác thực Supabase JWT và áp dụng role policy.  
- *Luồng đơn/tồn*: PostgreSQL transaction giữ, nhả hoặc commit tồn và lưu inventory operation.  
- *Luồng webhook bất đồng bộ*: API nhận payload từ simulator và gửi vào SQS FIFO; endpoint webhook bên ngoài kiểm tra chữ ký HMAC, còn thao tác simulator trên giao diện yêu cầu quyền Manager/Admin. Background worker trong cùng ứng dụng poll queue, xử lý và xóa message khi thành công. DLQ cô lập message lỗi theo redrive policy.
- *Luồng hủy đơn*: Sự kiện `order.cancelled` cập nhật trạng thái, lưu lý do hủy và nhả tồn đang giữ nếu đơn chưa xuất kho. Idempotency ngăn việc hoàn tồn hai lần.
- *Database và identity*: Supabase cung cấp PostgreSQL/Auth; dữ liệu nằm trong `public` schema của một tổ chức.  
- *AWS runtime*: Execution role cho phép ECS lấy image, gửi log và đọc secret được tham chiếu trong task definition. Task role cấp quyền SQS cho mã ứng dụng. Backend kết nối Supabase qua NAT Gateway; simulator chạy trong ứng dụng.
- *CI/CD*: GitHub Actions dùng OIDC để gọi AWS STS, nhận credential tạm thời, đẩy image lên ECR, cập nhật ECS service, đồng bộ frontend lên S3 và tạo CloudFront invalidation.

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
Dự toán được lập bằng AWS Pricing Calculator cho Region **Asia Pacific (Singapore) / ap-southeast-1**. Các mức dưới đây là chi phí cho một tháng theo cấu hình và lượng sử dụng demo đã nhập vào Calculator. Hóa đơn thực tế có thể khác do lưu lượng sử dụng, Free Tier và thuế.

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
| AWS WAF | 14.00 |
| **Tổng chi phí tháng** | **123.34** |

Chi phí SQS giả định `100.000` request FIFO có tính phí mỗi tháng với đơn giá `0,50 USD/triệu request`, chưa trừ Free Tier. Calculator cho kết quả **$0.00 chi phí trả trước**. Tổng dự toán một tháng cho cấu hình đầy đủ gồm hai NAT Gateway và AWS WAF là **$123.34**, chưa gồm thuế.

*Phương án tiết kiệm chi phí*
Môi trường học tập có thể triển khai một NAT Gateway và không sử dụng AWS WAF. Chi phí NAT Gateway khi đó còn khoảng **$43.07/tháng** và tổng dự toán giảm xuống **$66.27/tháng**, tiết kiệm khoảng **$57.07/tháng**. Phương án này chấp nhận NAT Gateway trở thành single point of failure và không còn lớp bảo vệ ứng dụng của AWS WAF.

Workshop tạo hai CloudWatch metric alarm cho ALB unhealthy target và SQS dead-letter queue. Chi phí monitoring được theo dõi trong mục Amazon CloudWatch, không tách thêm dòng dịch vụ. Giữ dự toán đã chốt và đối chiếu số thực tế trong AWS Billing sau demo. Bài lab không gắn SNS notification.

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
