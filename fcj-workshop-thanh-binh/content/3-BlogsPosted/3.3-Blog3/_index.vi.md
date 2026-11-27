---
title: "Blog 3"
date: 2026-11-27
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# TRIỂN KHAI .NET REST API TRÊN ECS FARGATE: TỪ DOCKER IMAGE ĐẾN CONTAINER ĐANG CHẠY

Trong NexusOMS, nhóm đóng gói .NET REST API thành Docker image và triển khai bằng Amazon ECS Fargate. ECS điều phối vòng đời task; Fargate cung cấp tài nguyên chạy container mà nhóm không phải quản lý máy chủ EC2.

### Luồng triển khai

1. GitHub Actions build Docker image từ source backend.
2. Pipeline gắn tag theo phiên bản và push image lên Amazon ECR.
3. Pipeline đăng ký Task Definition revision mới rồi cập nhật ECS Service.
4. Fargate khởi tạo task và pull image từ ECR.
5. ALB kiểm tra health check trước khi chuyển traffic tới task mới.
6. CloudWatch Logs nhận log để theo dõi quá trình khởi động và xử lý lỗi.

### Các điểm chính

- **Task Definition** mô tả image, CPU, memory, container port, biến môi trường, secret, log và IAM role.
- **ECS Service** duy trì số task mong muốn. Kiến trúc hai AZ cần hai task đang chạy, mỗi AZ một task; bài lab mặc định dùng một task.
- **Networking:** ALB dùng hai public subnet; Fargate chạy trong private subnet với `awsvpc`, network interface riêng và target group kiểu `ip`.
- **IAM:** execution role phục vụ pull image, lấy secret và gửi log; task role cấp quyền cho code gọi AWS API như SQS.
- **Secrets Manager** lưu connection string và webhook signing secret để ECS đưa vào container khi khởi tạo task.

### Bài học rút ra

Push image thành công chưa đồng nghĩa backend đã phục vụ được request. Cần kiểm tra revision mà service đang dùng, task `RUNNING`, target `Healthy` và phản hồi API. Khi bật deployment circuit breaker cùng rollback, ECS có thể phục hồi về deployment thành công trước đó nếu deployment mới thất bại.

### Nguồn tham khảo

- [Task Definition cho Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/fargate-tasks-services.html)
- [Tổng quan Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)
- [Amazon ECS Task Definitions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html)
- [Tổng quan Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)

Bài viết đã được duyệt và đăng trên AWS Study Group.

[Xem bài viết trên Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2350934632338162)

![Bài viết triển khai .NET REST API trên ECS Fargate đã được duyệt](/images/3-Blog/blog3.png)
