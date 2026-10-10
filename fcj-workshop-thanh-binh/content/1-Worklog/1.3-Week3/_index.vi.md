---
title: "Worklog Tuần 3"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---


### Mục tiêu Tuần 3:

* Ôn tập và củng cố toàn bộ kiến thức các dịch vụ AWS đã học ở các tuần trước.
* Tìm hiểu chuyên sâu các dịch vụ bảo mật, giám sát và kết nối mạng nâng cao trên AWS.
* Phân tích các Architecture Diagram thực tế từ cộng đồng FCJ để định hướng đề tài Workshop.
* Thực hành các bài lab về tự động hóa, observability và kết nối mạng.

### Các công việc trong tuần:

| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                                                                                                                                                                                                                                 |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1   | - Ôn tập toàn bộ dịch vụ AWS đã học (Compute, Storage, Networking, Database, Security) <br> - Phân tích các Architecture Diagram từ cộng đồng FCJ để đánh giá mức độ sử dụng và tầm quan trọng của từng dịch vụ <br> - Nghiên cứu yêu cầu và tiêu chí chấm điểm Workshop của FCJ                                                                                                                                                                                                                                                                                      | 28/09/2026   | 28/09/2026      | <https://cloudjourney.awsstudygroup.com/> <br> <https://hcm-rules.awsfcaj.com/3-project/> <br> <https://github.com/thienluhoan/fcj-workshop-template>                                                                                                              |
| 2   | - **Thực hành:** <br>&emsp; + Lambda Automation: tự động bật/tắt EC2 instance theo lịch để tối ưu chi phí <br>&emsp; + CloudWatch nâng cao + Grafana: tích hợp dashboard Grafana với CloudWatch metrics <br>&emsp; + Workshop CloudWatch nâng cao: custom metrics, log insights, composite alarms <br>&emsp; + Tags & Resource Groups: xây dựng tagging strategy chuẩn hóa cho tài nguyên AWS <br>&emsp; + IAM + Resource Tags (ABAC): kiểm soát quyền truy cập dựa trên tag tài nguyên <br>&emsp; + AWS Systems Manager: Run Command, Session Manager, Patch Manager | 29/09/2026   | 29/09/2026      | <https://000022.awsstudygroup.com/vi/> <br> <https://000029.awsstudygroup.com/vi/> <br> <https://000036.awsstudygroup.com/vi/> <br> <https://000027.awsstudygroup.com/vi/> <br> <https://000028.awsstudygroup.com/vi/> <br> <https://000031.awsstudygroup.com/vi/> |
| 3   | - **Thực hành:** <br>&emsp; + Amazon SSO (IAM Identity Center): thiết lập Single Sign-On cho AWS Organization (chỉ đọc - yêu cầu nâng cấp tài khoản) <br>&emsp; + IAM Permission Boundary: giới hạn quyền tối đa để ngăn leo thang đặc quyền <br>&emsp; + IAM Role Chaining với Condition: giới hạn assume role theo IP, thời gian, MFA <br>&emsp; + AWS Security Hub: đánh giá tiêu chuẩn bảo mật tự động (CIS, PCI DSS, AWS Foundational) <br>&emsp; + AWS WAF: bảo vệ ứng dụng web và API khỏi SQLi, XSS, DDoS Layer 7                                             | 01/10/2026   | 01/10/2026      | <https://000012.awsstudygroup.com/vi/> <br> <https://000030.awsstudygroup.com/vi/> <br> <https://000044.awsstudygroup.com/vi/> <br> <https://000018.awsstudygroup.com/vi/> <br> <https://000026.awsstudygroup.com/vi/>                                             |
| 4   | - **Thực hành:** <br>&emsp; + AWS KMS: tạo và quản lý khóa mã hóa, tích hợp với S3/EBS/RDS <br>&emsp; + AWS Backup: thiết lập kế hoạch sao lưu tự động với retention policy cho các tài nguyên AWS <br>&emsp; + VPC Peering: kết nối trực tiếp 2 VPC qua private IP <br>&emsp; + AWS Transit Gateway: quản lý tập trung nhiều kết nối VPC theo mô hình hub-and-spoke <br> - Tạo tài khoản AWS mới sau khi tài khoản cũ bị expire token bất ngờ                                                                                                                        | 03/10/2026   | 03/10/2026      | <https://000033.awsstudygroup.com/vi/> <br> <https://000013.awsstudygroup.com/vi/> <br> <https://000019.awsstudygroup.com/vi/> <br> <https://000020.awsstudygroup.com/vi/>                                                                                         |
| 5   | - Đọc và tìm hiểu lý thuyết (không thực hành do giới hạn Free Tier): <br>&emsp; + AWS File Storage Gateway: mở rộng lưu trữ on-premises lên S3 qua NFS/SMB <br>&emsp; + Amazon FSx for Windows: shared file storage cho môi trường Windows <br>&emsp; + Data Lake trên AWS: kiến trúc S3 → Glue Crawler → Data Catalog → Athena <br> - **Thực hành:** <br>&emsp; + Amazon DynamoDB nâng cao: Global Tables, DynamoDB Streams, DAX, TTL, backup & restore <br> - Tự đọc Architecture Diagram và tự khởi tạo tài nguyên trước khi dò lại với bài lab                    | 04/10/2026   | 04/10/2026      | <https://000024.awsstudygroup.com/vi/> <br> <https://000025.awsstudygroup.com/vi/> <br> <https://000035.awsstudygroup.com/vi/> <br> <https://000039.awsstudygroup.com/vi/>                                                                                         |


### Kết quả đạt được Tuần 3:

* Củng cố toàn diện kiến thức các nhóm dịch vụ AWS đã học và xác định được những dịch vụ quan trọng nhất cho Workshop cuối kỳ.

* Nắm vững các pattern tự động hóa và giám sát nâng cao:
  * Dùng Lambda tự động bật/tắt EC2 theo lịch - kỹ thuật tối ưu chi phí thực tế
  * Xây dựng dashboard Grafana tích hợp CloudWatch để quan sát hệ thống trực quan hơn
  * Tạo custom metrics, log insights queries và composite alarms trong CloudWatch

* Hiểu sâu về bảo mật AWS theo nhiều lớp:
  * Lớp ngoài: AWS WAF chặn SQLi, XSS, rate-based attack
  * Lớp mạng: Security Group và NACL
  * Lớp định danh: IAM Permission Boundary và Role Chaining với Condition (ABAC)
  * Lớp tuân thủ: AWS Security Hub tự động đánh giá theo chuẩn CIS và PCI DSS

* Nắm được các pattern kết nối mạng nâng cao:
  * VPC Peering cho kết nối trực tiếp point-to-point giữa 2 VPC
  * AWS Transit Gateway làm hub trung tâm quản lý nhiều VPC ở quy mô lớn
  * Rút ra được: Transit Gateway phù hợp hơn VPC Peering khi cần kết nối nhiều VPC

* Xây dựng được tagging strategy chuẩn hóa (Project, Environment, Owner) phục vụ quản lý tài nguyên và phân bổ chi phí.

* Phân tích 8 Architecture Diagram thực tế từ cộng đồng FCJ và xác định được các pattern phổ biến nhất:
  * S3 + CloudFront (phân phối frontend)
  * Lambda + API Gateway (serverless backend)
  * ECS Fargate + RDS (ứng dụng container hóa)

* Thể hiện tiến bộ rõ rệt về kỹ năng: có thể tự đọc Architecture Diagram và tự khởi tạo tài nguyên AWS từ đầu trước khi dò lại với bài lab - cải thiện đáng kể so với Tuần 1.

* Xử lý thành công sự cố tài khoản AWS bị expire token bất ngờ bằng cách tạo tài khoản mới và lấy lại $200 credit bonus.