---
title: "Các bài blogs đã đăng"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Phần này tổng hợp các bài viết của nhóm đã được duyệt trên [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj).

### [Blog 1 - GITHUB ACTIONS DEPLOY LÊN AWS KHÔNG DÙNG ACCESS KEY: OIDC HOẠT ĐỘNG NHƯ THẾ NÀO?](3.1-Blog1/)

Bài viết trình bày cách GitHub Actions sử dụng OIDC và AWS STS để nhận credential tạm thời, từ đó triển khai ứng dụng lên AWS mà không lưu access key dài hạn trong GitHub Secrets.

### [Blog 2 - ECS FARGATE KHÔNG KHỞI ĐỘNG: BÀI HỌC PHÂN BIỆT TASK ROLE VÀ EXECUTION ROLE](3.2-Blog2/)

Bài viết phân tích lỗi ECS Fargate không thể lấy secret khi khởi động, từ đó làm rõ trách nhiệm của Task Execution Role, Task Role và cách đọc thông báo AccessDenied để sửa đúng IAM policy.

###  [Blog 3 - ...](3.3-Blog3/)
Blog này giới thiệu Amazon EKS Pod Identity vừa bổ sung tính năng session policies, cho phép bạn thu hẹp quyền IAM một cách linh hoạt và chính xác cho từng pod mà không cần tạo thêm nhiều IAM roles riêng biệt. Đây là bước tiến quan trọng giúp áp dụng nguyên tắc least privilege hiệu quả hơn trong môi trường Kubernetes quy mô lớn.
