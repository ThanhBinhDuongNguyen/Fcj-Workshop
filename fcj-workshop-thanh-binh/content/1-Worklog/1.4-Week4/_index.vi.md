---
title: "Worklog Tuần 4"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---


### Mục tiêu Tuần 4:

* Tìm hiểu các chiến lược tối ưu chi phí AWS và áp dụng vào thiết kế project sau này.
* Nắm vững containerization và CI/CD pipeline với Docker, ECS và CodePipeline.
* Nghiên cứu các mô hình kiến trúc phần mềm: Monolith, Microservices và Domain-Driven Design.
* Xây dựng hoàn chỉnh ứng dụng full-stack serverless sử dụng Lambda, DynamoDB, S3 và Cognito.

### Các công việc trong tuần:

| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | - Tìm hiểu tối ưu chi phí AWS (đọc lý thuyết, không thực hành): <br>&emsp; + Savings Plans (Compute vs EC2 Instance) vs Reserved Instance vs Reserved DB Instance <br>&emsp; + EC2 Resource Optimization: tiêu chí lựa chọn instance type phù hợp <br>&emsp; + Trực quan hóa chi phí qua Cost Explorer và CloudWatch                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | 05/10/2026   | 05/10/2026      | <https://000042.awsstudygroup.com/vi/> <br> <https://000032.awsstudygroup.com/vi/> <br> <https://000034.awsstudygroup.com/vi/>                                                                                                                                                                                                                                                                                                                            |
| 2   | - **Thực hành:** <br>&emsp; + Triển khai ứng dụng với Docker: build image, push lên Docker Hub/ECR bằng CLI, chạy container <br>&emsp; + Triển khai trên Amazon ECS: tạo Task Definition, Service và Cluster <br>&emsp; + AWS CodePipeline: thiết lập CI/CD pipeline tự động (GitHub → CodeBuild → CodeDeploy) <br>&emsp; + Tự động hóa triển khai với CodePipeline <br> - Tự tìm hiểu CodePipeline qua YouTube và bài viết online (GitHub repo gốc của lab đã bị gỡ)                                                                                                                                                                                                                                                                                                                                                      | 06/10/2026   | 06/10/2026      | <https://000015.awsstudygroup.com/vi/> <br> <https://fcj-dntu.github.io/000016-deploy-app-ecs/> <br> <https://000017.awsstudygroup.com/vi/> <br> <https://000023.awsstudygroup.com/vi/> <br> YouTube, bài viết online                                                                                                                                                                                                                                     |
| 3   | - Đọc và phân tích kiến trúc (không thực hành - lab yêu cầu Windows instance + Eclipse + Java): <br>&emsp; + Các pattern chuyển đổi từ Monolith sang Microservice trên AWS <br>&emsp; + Chiến lược phát hành tự động: Blue/Green, Canary deployment <br> - Tự nghiên cứu qua sách kiến trúc: <br>&emsp; + *Building Microservices* (Sam Newman) <br>&emsp; + *Learning Domain-Driven Design* (Vladik Khononov) <br>&emsp; + *Monolith to Microservices* (Sam Newman)                                                                                                                                                                                                                                                                                                                                                       | 07/10/2026   | 07/10/2026      | <https://000050.awsstudygroup.com/vi/> <br> <https://000051.awsstudygroup.com/vi/> <br> Building Microservices - Sam Newman <br> Learning Domain-Driven Design - Vladik Khononov <br> Monolith to Microservices - Sam Newman                                                                                                                                                                                                                              |
| 4   | - Đọc và tìm hiểu (không thực hành - lab yêu cầu Windows instance, lỗi template): <br>&emsp; + Tạo một Microservice trên AWS <br>&emsp; + Cơ cấu lại dữ liệu và quy trình làm việc trong kiến trúc Microservice <br>&emsp; + Microservice Messaging & Eventing (SQS/SNS/EventBridge) <br>&emsp; + Xác thực Single Page Application <br>&emsp; + Các dịch vụ AI trên AWS (Bedrock, Rekognition, Comprehend) <br>&emsp; + AWS Step Functions: orchestration workflow serverless                                                                                                                                                                                                                                                                                                                                              | 08/10/2026   | 08/10/2026      | <https://000052.awsstudygroup.com/vi/> <br> <https://000053.awsstudygroup.com/vi/> <br> <https://000054.awsstudygroup.com/vi/> <br> <https://000055.awsstudygroup.com/vi/> <br> <https://000056.awsstudygroup.com/vi/> <br> <https://000047.awsstudygroup.com/vi/>                                                                                                                                                                                        |
| 5-6 | - **Thực hành (chuỗi Serverless - 9 lab):** <br>&emsp; + Backend Serverless: Lambda + S3 + DynamoDB - xây dựng API CRUD hoàn chỉnh <br>&emsp; + Frontend cho API Serverless - tích hợp React với Lambda backend <br>&emsp; + AWS SAM - Infrastructure as Code cho triển khai serverless <br>&emsp; + Amazon Cognito - xác thực người dùng: Login, Register, Change Password <br>&emsp; + SQS + SNS - xử lý sự kiện và gửi email thông báo tự động <br>&emsp; + Giám sát Serverless - CloudWatch logs, metrics, X-Ray tracing <br>&emsp; + AWS AppSync + GraphQL API <br>&emsp; + CI/CD cho Serverless (hoàn thành 90% - source đã clear trước khi làm lab) <br> - Đọc lý thuyết (giới hạn Free Tier): Custom Domain + SSL với Route 53 <br> - **Xây dựng hoàn thiện website bán sách** với đầy đủ chức năng chỉ dùng AWS | 09/10/2026   | 10/10/2026      | <https://000078.awsstudygroup.com/vi/> <br> <https://000079.awsstudygroup.com/vi/> <br> <https://000080.awsstudygroup.com/vi/> <br> <https://000081.awsstudygroup.com/vi/> <br> <https://000082.awsstudygroup.com/vi/> <br> <https://000083.awsstudygroup.com/vi/> <br> <https://000084.awsstudygroup.com/vi/> <br> <https://000085.awsstudygroup.com/vi/> <br> <https://000086.awsstudygroup.com/vi/> <br> <https://www.youtube.com/watch?v=z7nLsJvEyMY> |


### Kết quả đạt được Tuần 4:

* Nắm vững các chiến lược tối ưu chi phí AWS và tiêu chí lựa chọn EC2 instance type trong thực tế:
  * CPU/RAM: ước tính workload thực tế - t3.micro/t3.small đủ dùng cho web app nhỏ
  * Network: chọn instance có enhanced networking cho ứng dụng cần băng thông cao
  * Storage: chọn instance tối ưu storage (io1/io2) khi cần I/O cao
  * Mô hình chi phí: On-Demand cho dev/test; Reserved/Savings Plans cho production ổn định
  * Savings Plans giảm tới 66% chi phí so với On-Demand khi cam kết 1–3 năm

* Nắm vững quy trình containerization và CI/CD:
  * Hiểu rõ luồng Docker: viết code → build image → tag → push lên ECR/Docker Hub → ECS pull và chạy
  * Docker Image là artifact bất biến đảm bảo nhất quán giữa môi trường dev và production
  * ECS quản lý vòng đời container trên AWS - thay thế việc tự quản lý Docker trên EC2
  * Tự tìm hiểu CodePipeline qua tài liệu bên ngoài khi repo gốc của lab bị gỡ

* Nghiên cứu chuyên sâu các mô hình kiến trúc phần mềm qua sách kỹ thuật:
  * Modular Monolith là điểm khởi đầu tốt hơn việc chuyển thẳng sang Microservice
  * DDD giúp xác định đúng ranh giới service (Bounded Context) - tránh phân tách sai gây coupling
  * Strangler Fig Pattern cho phép chuyển đổi Monolith tiệm tiến mà không làm gián đoạn hệ thống
  * Định hướng chuyển các bài lab sang .NET (thay vì Java) với sự hỗ trợ của AI

* **Thành tích nổi bật: Xây dựng hoàn thiện website bán sách serverless chỉ dùng AWS:**
  * Frontend: React
  * Backend: Lambda + DynamoDB + S3
  * Xác thực: Amazon Cognito (Login, Register, Change Password)
  * Thông báo: Gửi email tự động qua SES + Lambda
  * Tự xử lý toàn bộ lỗi phát sinh: sai kiểu dữ liệu, sai format YAML, cấu hình biến môi trường, đọc hiểu code Python Lambda
  * Luyện tập sâu Lambda trigger: S3 upload → Lambda, DynamoDB Stream → Lambda sync
  * AWS SAM giúp deploy nhanh hơn đáng kể so với tạo thủ công từng resource
  * Hoàn thành clean up toàn bộ tài nguyên AWS sau mỗi lab

* Thể hiện kỹ năng tự giải quyết vấn đề tốt: toàn bộ lỗi phát sinh đều tự research và xử lý được mà không cần hỗ trợ từ bên ngoài.

* Xác định 2 nội dung cần bổ sung trong Workshop project:
  * Custom Domain + SSL (Route 53 + ACM) - bỏ qua do chi phí mua domain
  * CI/CD pipeline - đạt 90%; source code đã clear trước khi làm lab nên cần setup lại