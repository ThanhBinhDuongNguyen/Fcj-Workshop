---
title: "Blog 2"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---
# ECS FARGATE KHÔNG KHỞI ĐỘNG: BÀI HỌC PHÂN BIỆT TASK ROLE VÀ EXECUTION ROLE

Khi đưa backend ASP.NET Core của NexusOMS lên ECS Fargate, service đã được tạo nhưng task liên tục dừng với lỗi:

> `ResourceInitializationError ... AccessDeniedException ... secretsmanager:GetSecretValue`

Lỗi này giúp nhóm phân biệt rõ trách nhiệm của hai IAM role trong ECS.

### Hai role khác nhau ở đâu?

- **Task Execution Role** được ECS agent sử dụng để kéo image từ Amazon ECR, gửi log và lấy secret được tham chiếu trong task definition.
- **Task Role** được mã ứng dụng sử dụng khi gọi AWS API trong thời gian chạy, chẳng hạn nhận và xử lý message từ Amazon SQS.

Trong NexusOMS, connection string được lưu trong AWS Secrets Manager và được task definition đưa vào biến môi trường. ECS cần lấy secret trước khi container khởi động, nên quyền `secretsmanager:GetSecretValue` phải được cấp cho execution role. Chỉ thêm quyền này vào task role sẽ không xử lý được lỗi khởi tạo.

### Cách xác định đúng policy

Khi đọc lỗi IAM, nhóm tập trung vào bốn câu hỏi:

1. Danh tính nào bị từ chối?
2. Hành động nào không được phép?
3. Tài nguyên nào đang được truy cập?
4. Lời gọi xảy ra trước khi container khởi động hay trong lúc ứng dụng chạy?

Thông báo lỗi đã chỉ rõ execution role, hành động `GetSecretValue` và ARN của secret. Nhờ đó, nhóm cấp quyền đọc đúng secret thay vì dùng `AdministratorAccess`. Nếu secret dùng customer managed KMS key, role còn cần quyền giải mã phù hợp.

### Kiểm tra sau khi sửa

Sửa quyền IAM chỉ giúp task vượt qua bước lấy secret. Nhóm tiếp tục kiểm tra:

- Task có ở trạng thái `RUNNING` không?
- Target group có báo `Healthy` không?
- Endpoint có phản hồi không?
- Các chức năng dùng database có hoạt động đúng không?

Sau khi lỗi IAM được xử lý, hệ thống từng tiếp tục trả HTTP 500 do connection string sai định dạng. Đây là lỗi cấu hình ứng dụng, không còn là lỗi quyền. Bài học chính là phải xác định **ai đang gọi**, **gọi hành động gì**, **trên tài nguyên nào** và **ở thời điểm nào** trước khi thêm IAM policy.

### Nguồn tham khảo

- [IAM for Amazon ECS on AWS Fargate — Wilklins Nyatteng](https://dev.to/aws-builders/iam-for-amazon-ecs-on-aws-fargate-2bk8)
- [Amazon ECS task execution IAM role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html)

### Bài viết đã đăng

[Xem bài viết trên Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2347546992676926)

![The ECS Fargate article published in AWS Study Group](/images/3-Blog/blog2.png)
