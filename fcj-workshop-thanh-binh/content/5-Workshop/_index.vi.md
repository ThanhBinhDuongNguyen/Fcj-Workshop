---
title: "Workshop"
date: 2026-11-15
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

Triển khai NexusOMS trên hạ tầng AWS gồm hai Availability Zone, xử lý webhook mô phỏng bất đồng bộ bằng SQS FIFO và dùng GitHub Actions để cập nhật backend cùng frontend. Bài lab mặc định chạy một Fargate task; tăng lên hai task để trình diễn cấu hình hai AZ.

> Thay mọi placeholder `<...>` bằng giá trị thật. Các giá trị cần nhập được đặt trong khối code để có thể sao chép.

## Nội dung

1. [Tổng quan workshop](./5.1-Workshop-overview/)
2. [Chuẩn bị mã nguồn và tài khoản](./5.2-Prerequisite/)
3. [Tạo hạ tầng AWS](./5.3-Infrastructure/)
4. [Triển khai ứng dụng](./5.4-Application-deployment/)
5. [Thiết lập CI/CD bằng GitHub Actions](./5.5-CICD/)
6. [Kiểm thử và xác minh](./5.6-Verification/)
7. [Dọn dẹp tài nguyên](./5.7-Cleanup/)
