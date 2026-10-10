---
title: "Worklog Tuần 1"
date: 2026-09-28
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu Tuần 1:

* Giao lưu, kết nối với các thành viên First Cloud AI Journey và nắm rõ nội quy thực tập.
* Học các kiến thức nền tảng về điện toán đám mây, cách quản lý tài khoản và sử dụng dịch vụ AWS cơ bản.
* Làm quen với AWS Management Console, AWS CLI, Amazon EC2 và Amazon S3.

### Các công việc thực hiện trong tuần:
| Ngày  | Công việc                                                                                                                                                                                                                                                                  | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                      |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------------------- |
| Thứ 2 | - Tạo tài khoản AWS, tìm hiểu cấu trúc account và phân quyền cơ bản <br> - Thiết lập cảnh báo chi phí với AWS Budgets (billing alert $10/tháng) <br> - Tìm hiểu hệ thống hỗ trợ kỹ thuật AWS Support và IAM (Users, Groups, Policies)                                      | 15/09/2026   | 15/09/2026      | https://cloudjourney.awsstudygroup.com/ |
| Thứ 3 | - Tìm hiểu kiến thức mạng cơ bản với Amazon VPC (Subnet, IGW, Route Table, Security Group) <br> - Học kiến thức máy chủ ảo Amazon EC2 (Launch instance, key pair, SSH connection) <br> - **Thực hành:** Tạo VPC, khởi tạo EC2 Linux và SSH vào instance thành công         | 16/09/2026   | 16/09/2026      | https://cloudjourney.awsstudygroup.com/ |
| Thứ 4 | - Học về IAM Roles cho EC2, AWS Cloud9 (IDE trên Cloud) <br> - Học về Hosting website tĩnh trên Amazon S3 và tổng quan cơ sở dữ liệu Amazon RDS <br> - **Thực hành:** Gán IAM Role, host web tĩnh trên S3, kết nối RDS và luyện tập các lệnh Linux CLI (cat, echo, vi/vim) | 17/09/2026   | 17/09/2026      | https://cloudjourney.awsstudygroup.com/ |
| Thứ 5 | - Tìm hiểu về AWS CLI, Amazon Lightsail và Lightsail Containers <br> - **Thực hành:** Cấu hình AWS CLI từ terminal, khởi tạo và triển khai container trên Amazon Lightsail                                                                                                 | 18/09/2026   | 18/09/2026      | https://cloudjourney.awsstudygroup.com/ |
| Thứ 6 | - Tìm hiểu EC2 Auto Scaling, Amazon CloudWatch, Route 53 và DynamoDB <br> - **Thực hành:** Tạo Auto Scaling Group, cấu hình CloudWatch Alarm, thiết lập Route 53 và thao tác CRUD trên DynamoDB qua AWS CloudShell                                                         | 19/09/2026   | 19/09/2026      | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được trong Tuần 1:

* Hiểu rõ khái niệm đám mây, tầm quan trọng của việc đặt Billing Alert ngay từ đầu để tránh phát sinh chi phí bất ngờ.
* Khởi tạo và cấu hình thành công AWS CLI, sử dụng linh hoạt công cụ dòng lệnh (Linux CLI & CloudShell) song song với AWS Console.
* Nắm vững kiến thức cốt lõi và thực hành thành công triển khai hạ tầng với VPC, EC2, S3 Static Hosting, RDS và DynamoDB.
* Rút ra bài học thực tế: Thao tác trên Linux CLI hiệu quả và mượt mà hơn nhiều so với việc chạy Windows Server trên EC2 cấu hình thấp.