---
title: "Blog 3"
date: 2026-11-27
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# TRIỂN KHAI .NET REST API TRÊN ECS FARGATE: TỪ DOCKER IMAGE ĐẾN CONTAINER ĐANG CHẠY

### Giới thiệu

Trong NexusOMS, nhóm đóng gói .NET REST API thành Docker image và triển khai bằng Amazon ECS Fargate. Bài viết trình bày quá trình từ build image đến khi container phục vụ request, với ECS điều phối task và Fargate cung cấp tài nguyên mà nhóm không phải quản lý máy chủ EC2.

### Nội dung chính

1. **Build và push image:** GitHub Actions build backend, gắn tag theo phiên bản và đẩy image lên Amazon ECR.
2. **Task Definition:** Pipeline đăng ký revision mới, mô tả image, CPU, memory, port, biến môi trường, secret, log và IAM role.
3. **ECS Service:** Service cập nhật revision và duy trì số task mong muốn. Kiến trúc hai AZ cần hai task, mỗi AZ một task; bài lab mặc định dùng một task.
4. **Networking và health check:** ALB dùng hai public subnet. Fargate chạy trong private subnet với awsvpc, network interface riêng và target group kiểu ip. ALB kiểm tra health trước khi chuyển traffic tới task mới.
5. **Quyền và cấu hình:** Execution role phục vụ pull image, lấy secret và gửi log; task role cấp quyền SQS cho code. Secrets Manager cung cấp connection string và webhook signing secret khi task khởi tạo.

### Bài học rút ra

Push image thành công chưa đồng nghĩa API đã hoạt động. Cần kiểm tra Task Definition revision, task RUNNING, target Healthy và phản hồi API; dùng CloudWatch Logs để điều tra lỗi. Khi bật circuit breaker và rollback, ECS có thể trở về deployment thành công trước đó nếu bản mới thất bại.

### Nguồn tham khảo

- [Task Definition cho Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/fargate-tasks-services.html)
- [Tổng quan Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)
- [Amazon ECS Task Definitions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html)
- [Tổng quan Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)

### Bài viết và minh chứng

Bài viết đã được duyệt và đăng trên AWS Study Group.

[Xem bài viết trên Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2350934632338162)

![Minh chứng bài Blog 3 đã đăng trên AWS Study Group](/images/3-Blog/blog3.png)
