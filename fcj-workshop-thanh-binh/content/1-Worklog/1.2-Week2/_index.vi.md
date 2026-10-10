---
title: "Worklog Tuần 2"
date: 2026-09-28
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu Tuần 2:

* Đi sâu vào kiến trúc mạng AWS Networking Workshop, CDN CloudFront, Lambda@Edge và Workload Windows.
* Tổng hợp, phân tích các mẫu sơ đồ kiến trúc (Architecture Diagrams) thực tế từ cộng đồng FCJ.
* Tìm hiểu quy trình chuyển dịch hạ tầng (Migration) và tham dự sự kiện Buildrathon Kick-off.

### Các công việc thực hiện trong tuần:
| Ngày  | Công việc                                                                                                                                                                                    | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                       |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ---------------------------------------- |
| Thứ 2 | - Tham gia AWS Networking Workshop (VPC, Subnet, Routing) <br> - Tìm hiểu Amazon CloudFront (CDN), Lambda@Edge và cách triển khai ứng dụng Windows trên AWS                                  | 21/09/2026   | 21/09/2026      | https://cloudjourney.awsstudygroup.com/  |
| Thứ 3 | - Làm lại lab Windows on AWS với CloudFormation template mới <br> - Thực hành cấu hình AWS Managed Microsoft AD và thử nghiệm Amazon WorkSpaces                                              | 22/09/2026   | 22/09/2026      | https://cloudjourney.awsstudygroup.com/  |
| Thứ 4 | - Ôn tập tổng hợp nhóm dịch vụ: Compute, Storage, Networking và Management <br> - Đọc quy chế bài tập lớn, phân tích 8 sơ đồ kiến trúc (Architecture Diagrams) từ cộng đồng FCJ              | 23/09/2026   | 23/09/2026      | https://hcm-rules.awsfcaj.com/3-project/ |
| Thứ 5 | - Phân tích tần suất xuất hiện và đánh giá độ quan trọng của từng dịch vụ cho Workshop <br> - Xác định mô hình kiến trúc phù hợp nhất (S3 + CloudFront + Lambda + API Gateway / ECS Fargate) | 24/09/2026   | 24/09/2026      | https://hcm-rules.awsfcaj.com/3-project/ |
| Thứ 6 | - Tìm hiểu dịch vụ Migration: AWS VM Import/Export, AWS DMS và AWS SCT <br> - **Thực hành:** Import thành công file máy ảo on-premises (.vmdk) lên EC2 AMI via AWS CLI                       | 25/09/2026   | 25/09/2026      | https://cloudjourney.awsstudygroup.com/  |
| Thứ 7 | - Tham dự buổi **Buildrathon Kick-off: Code the Future with CMC Global** <br> - Nắm bắt yêu cầu, tiêu chí đánh giá, tư duy học tập và định hướng lại đề tài Workshop cuối kỳ                 | 26/09/2026   | 26/09/2026      | Sự kiện Buildrathon Kick-off FCJ         |

### Kết quả đạt được trong Tuần 2:

* Nắm rõ cách tối ưu hiệu năng và phân phối nội dung toàn cầu qua Amazon CloudFront & Lambda@Edge.
* Rút ra bài học quản lý chi phí đắt giá: Kiểm tra kỹ bảng giá dịch vụ trước khi bật (ví dụ: Amazon WorkSpaces tốn $6 cho 30 phút trải nghiệm do chọn sai gói).
* Lựa chọn được định hướng kiến trúc tối ưu cho bài Workshop dựa trên năng lực cá nhân và các bài mẫu thực tế từ cộng đồng.
* Thực hành thành công quá trình dịch chuyển máy ảo (VM Import/Export) lên cloud và thu thập đầy đủ thông tin định hướng từ sự kiện Buildrathon Kick-off.