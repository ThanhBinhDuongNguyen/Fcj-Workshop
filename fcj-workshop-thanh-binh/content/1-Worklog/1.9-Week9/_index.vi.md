---
title: "Worklog Tuần 9"
date: 2026-11-09
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---


### Mục tiêu tuần 9:

* Kiểm tra backend container và chuẩn bị triển khai lên AWS.
* Triển khai NexusOMS lên AWS (backend trên ECS Fargate, frontend trên S3 và CloudFront) và xử lý các lỗi triển khai.
* Thiết lập CI/CD với GitHub Actions và AWS OIDC.
* Hoàn thiện tài liệu workshop song ngữ và sơ đồ kiến trúc high availability.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                             |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | -------------------------------------------------------------------------- |
| 2 | - Sửa Docker image backend bằng cách bổ sung thư viện Kerberos cho Npgsql <br> - Kiểm tra khả năng chạy backend container khi kết nối PostgreSQL/Supabase                                                                                                                                                                                                                                                                                                         | 09/11/2026   | 09/11/2026      | [Repository NexusOMS](https://github.com/sneakez/nexus-aws) <br> [.NET app with Docker](https://learn.microsoft.com/en-us/dotnet/core/docker/build-container) <br> [Npgsql security and encryption](https://www.npgsql.org/doc/security.html) |
| 3 | - Rà soát kiến trúc triển khai AWS, chi phí dự kiến và luồng triển khai frontend/backend <br> - Chuẩn bị tài nguyên và hướng dẫn demo                                                                                                                                                                                                                                                                                                                             | 10/11/2026   | 10/11/2026      | [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/framework.html) <br> [AWS Pricing Calculator](https://calculator.aws/) |
| 4 | - Cập nhật dependencies frontend <br> - Tiếp tục hoàn thiện Proposal và kế hoạch demo AWS                                                                                                                                                                                                                                                                                                                                                                         | 11/11/2026   | 11/11/2026      | [Repository NexusOMS](https://github.com/sneakez/nexus-aws) <br> Proposal NexusOMS |
| 5 | - Triển khai NexusOMS lên AWS và xử lý lỗi backend HTTP 500 <br> - Sửa session timeout ở frontend <br> - Sửa nghiệp vụ chọn sản phẩm theo tồn kho và bổ sung nhận số lượng tồn ban đầu khi tạo sản phẩm <br> - Đồng bộ và làm sạch repository, đẩy thay đổi lên các repository GitHub                                                                                                                                                                             | 12/11/2026   | 12/11/2026      | [Repository NexusOMS](https://github.com/sneakez/nexus-aws) <br> [Getting started with ECS Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/getting-started-fargate.html) |
| 6 | - Thực hành hạ tầng AWS: VPC, hai public subnet, hai private subnet, Internet Gateway, hai NAT Gateway, route table, security group, ALB, target group, ECS và Fargate <br> - Xử lý lỗi subnet, route IGW, security group, Secrets Manager, task role và health check <br> - Cleanup tài nguyên sau các lần triển khai lỗi                                                                                                                                        | 13/11/2026   | 13/11/2026      | [VPC with private subnets and NAT Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-example-private-subnets-nat.html) <br> [ECS service load balancing](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-load-balancing.html) <br> [Pass Secrets Manager secrets to ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/secrets-envvar-secrets-manager.html) |
| 7 | - Hoàn thiện tài liệu workshop NexusOMS song ngữ <br> - Chuẩn hóa tên file ảnh để hoạt động trên Vercel <br> - Bổ sung báo cáo sự kiện Cloud Mastery <br> - Chỉnh lại sơ đồ kiến trúc high availability: CloudFront, S3, ALB, ECS Fargate, ECR, NAT Gateway, S3 Gateway Endpoint, CloudWatch, CloudTrail, Supabase và tích hợp bên ngoài <br> - Thực hành đầy đủ CI/CD bằng GitHub Actions và AWS OIDC: tạo IAM role và inline policy cho GitHub Actions, cấu hình repository variables/secrets, build và push backend image lên ECR, cập nhật ECS service, deploy frontend lên S3 và invalidation CloudFront <br> - Sửa lỗi OIDC, Docker build, Secrets Manager và ALB health check <br> - Chỉnh workflow chỉ publish backend container image <br> - Tái cấu trúc nội dung và hình ảnh workshop theo luồng mới | 14/11/2026 | 15/11/2026 | Tài liệu workshop NexusOMS <br> [AWS Architecture Icons](https://aws.amazon.com/architecture/icons/) <br> [GitHub OIDC with AWS](https://docs.github.com/en/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-aws) <br> [Push an image to Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html) <br> [CloudFront invalidation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html) |


### Kết quả đạt được tuần 9:

* Chuẩn bị backend container: sửa Docker image bằng thư viện Kerberos cho Npgsql và xác nhận container kết nối được PostgreSQL/Supabase.

* Triển khai NexusOMS lên AWS:
  * Backend trên Amazon ECS với AWS Fargate
  * Frontend trên Amazon S3 và CloudFront
  * Xử lý lỗi backend HTTP 500, session timeout ở frontend và nghiệp vụ chọn sản phẩm theo tồn kho, đồng thời bổ sung số lượng tồn ban đầu khi tạo sản phẩm

* Tự dựng tầng mạng và compute trên AWS: VPC, hai public subnet và hai private subnet trên hai Availability Zone, Internet Gateway, hai NAT Gateway, route table, security group, ALB, target group, ECS và Fargate. Xử lý được các lỗi subnet, route IGW, security group, Secrets Manager, task role và health check.

* Thiết lập CI/CD từ GitHub Actions đến Amazon ECR và Amazon ECS/Fargate, và từ GitHub Actions đến S3 kèm invalidation CloudFront:
  * Dùng GitHub OIDC thay cho AWS access key dài hạn, với IAM role và inline policy cho GitHub Actions
  * Cấu hình repository variables và secrets
  * Sửa lỗi OIDC, Docker build, Secrets Manager và ALB health check
  * Chỉnh workflow chỉ publish backend container image

* Kiểm soát chi phí khi thử nghiệm bằng cách cleanup tài nguyên AWS sau các lần triển khai lỗi và sau demo.

* Hoàn thiện tài liệu:
  * Workshop NexusOMS song ngữ kèm ảnh hướng dẫn, tái cấu trúc theo luồng mới
  * Chuẩn hóa tên file ảnh để hoạt động trên Vercel
  * Sơ đồ kiến trúc high availability gồm CloudFront, S3, ALB, ECS Fargate, ECR, NAT Gateway, S3 Gateway Endpoint, CloudWatch, CloudTrail, Supabase và tích hợp bên ngoài
  * Báo cáo sự kiện Cloud Mastery

* Kết quả: NexusOMS chạy end-to-end trên AWS với kiến trúc hai Availability Zone và pipeline triển khai tự động.
