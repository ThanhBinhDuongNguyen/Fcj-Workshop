---
title: "Blog 2"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# ECS FARGATE KHÔNG KHỞI ĐỘNG: BÀI HỌC PHÂN BIỆT TASK ROLE VÀ EXECUTION ROLE

### Giới thiệu

Khi triển khai backend ASP.NET Core của NexusOMS lên ECS Fargate, service đã được tạo nhưng task liên tục dừng với lỗi ResourceInitializationError và AccessDenied đối với secretsmanager:GetSecretValue. Bài viết dùng tình huống này để phân biệt hai IAM role trong task definition.

### Nội dung chính

1. **Execution role:** ECS dùng role này để pull image từ ECR, gửi log và lấy secret được tham chiếu trong task definition.
2. **Task role:** Mã ứng dụng dùng role này khi gọi AWS API trong lúc chạy, chẳng hạn gửi và nhận message SQS.
3. **Xác định đúng quyền:** Connection string được ECS lấy từ Secrets Manager trước khi container khởi động, nên quyền GetSecretValue thuộc execution role. Nếu secret dùng customer managed KMS key, cần kiểm tra thêm quyền giải mã.
4. **Đọc thông báo lỗi:** Xác định danh tính bị từ chối, hành động, tài nguyên và thời điểm gọi. Cấp quyền trên đúng secret thay vì dùng AdministratorAccess.
5. **Kiểm chứng kết quả:** Theo dõi task thay thế, target Healthy và phản hồi API. Lỗi HTTP 500 do connection string sai định dạng cần được xử lý riêng với lỗi IAM.

### Bài học rút ra

Trạng thái service Active chưa chứng minh ứng dụng hoạt động. Trước khi thêm policy, cần biết ai đang gọi AWS, gọi hành động gì, trên tài nguyên nào và ở thời điểm nào. Cách đọc lỗi này giúp sửa đúng role và tránh cấp quyền quá rộng.

### Nguồn tham khảo

- [IAM for Amazon ECS on AWS Fargate — Wilklins Nyatteng](https://dev.to/aws-builders/iam-for-amazon-ecs-on-aws-fargate-2bk8)
- [Amazon ECS task execution IAM role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html)

### Bài viết và minh chứng

Bài viết đã được duyệt và đăng trên AWS Study Group.

[Xem bài viết trên Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2347546992676926)

![Minh chứng bài Blog 2 đã đăng trên AWS Study Group](/images/3-Blog/blog2.png)
