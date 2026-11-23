---
title: "Worklog Tuần 10"
date: 2026-11-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 10:

* Hoàn thiện cơ chế xử lý webhook bất đồng bộ cho NexusOMS bằng Amazon SQS FIFO và Dead-letter Queue.
* Triển khai, kiểm thử và xử lý lỗi phiên bản mới trên AWS ECS Fargate.
* Hoàn thiện nội dung Workshop, Proposal, sơ đồ kiến trúc và các phần báo cáo của chương trình.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | --------------- |
| 2 | - Thiết kế luồng xử lý webhook bất đồng bộ bằng Amazon SQS FIFO và Dead-letter Queue. <br> - Bổ sung cơ chế chạy local khi chưa cấu hình SQS trên AWS. <br> - Xử lý nghiệp vụ tạo đơn, hủy đơn và hoàn trả tồn kho từ webhook. <br> - Xây dựng giao diện mô phỏng webhook, gửi nhiều đơn và theo dõi tiến trình xử lý. <br> - Bổ sung chức năng kết thúc demo, xóa dữ liệu thử nghiệm và dữ liệu mẫu trên Supabase. <br> - Đồng bộ source code lên các repository, bao gồm repository sạch dành cho workshop. | 16/11/2026 | 16/11/2026 | <https://github.com/sneakez/nexus-aws> |
| 3 | - Triển khai và kiểm thử SQS cùng backend trên ECS Fargate. <br> - Xử lý lỗi webhook timeout, message vào DLQ và phản hồi rỗng từ SQS. <br> - Điều chỉnh dữ liệu demo để không làm sai tồn kho. <br> - Sửa lỗi xem chi tiết đơn hàng và trường dữ liệu boolean. <br> - Kiểm thử lại các luồng tạo đơn, hủy đơn và tải cao. <br> - Bổ sung hướng dẫn SQS, IAM policy, biến môi trường và bước dọn dẹp vào workshop. <br> - Cập nhật Proposal và sơ đồ kiến trúc với luồng xử lý bất đồng bộ. | 17/11/2026 | 17/11/2026 | <https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html> |
| 4 | - Rà soát workshop theo quá trình triển khai AWS thực tế. <br> - Chuẩn hóa phần chuẩn bị repository, GitHub Actions, OIDC và CI/CD. <br> - Chuyển hình ảnh sang cú pháp Markdown để dễ thay thế và hiển thị ổn định. <br> - Chuẩn hóa các lệnh PowerShell để người học có thể sao chép trực tiếp. <br> - Sắp xếp lại nội dung hướng dẫn và hình ảnh theo đúng thứ tự thao tác. | 18/11/2026 | 18/11/2026 | <https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/about-security-hardening-with-openid-connect> |
| 5 | - Chuẩn hóa hướng dẫn tạo IAM role, Secrets Manager và ECR repository. <br> - Cập nhật các phần Application Load Balancer, ECS cluster, Task Definition và ECS Service theo giao diện AWS mới. <br> - Hoàn thiện hướng dẫn S3 frontend, CloudFront Distribution và CloudFront Function cho React SPA. <br> - Sửa lỗi định dạng JSON, JavaScript và PowerShell trong nội dung workshop. <br> - Rà soát luồng CloudTrail, CloudWatch và S3 lưu access log. | 19/11/2026 | 19/11/2026 | <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html> |
| 6 | - Bổ sung các bước kiểm tra quyền repository và cấu hình OIDC cho GitHub Actions. <br> - Làm rõ các bước triển khai, kiểm tra kết quả và xử lý lỗi thường gặp. <br> - Bổ sung CloudWatch Alarm và hướng dẫn dọn dẹp tài nguyên sau workshop. <br> - Cập nhật nguồn clone chính sang repository `sneakez/nexus-aws`. <br> - Rà soát Proposal, chi phí vận hành theo tháng và phương án tiết kiệm chi phí. <br> - Kiểm tra nội dung tiếng Việt, tiếng Anh và quá trình build website. | 20/11/2026 | 20/11/2026 | <https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html> |
| 7 | - Tham gia AWS Student Community Day Vietnam 2026 tại văn phòng AWS. <br> - Viết và chỉnh sửa báo cáo sự kiện bằng tiếng Việt và tiếng Anh. <br> - Hoàn thiện Proposal với chi phí WAF, hai NAT Gateway và phương án tiết kiệm sử dụng một NAT Gateway, không dùng WAF. <br> - Hoàn thiện phần Self-assessment và Sharing & Feedback. <br> - Bổ sung bản tiếng Việt cho báo cáo Event 3. <br> - Rà soát thông tin cá nhân, nội dung mẫu và placeholder trên toàn bộ website. <br> - Kiểm tra lại câu chữ, liên kết điều hướng và khả năng build của website. | 21/11/2026 | 21/11/2026 |  |


### Kết quả đạt được tuần 10:

* Hoàn thiện cơ chế xử lý webhook bất đồng bộ bằng Amazon SQS FIFO và Dead-letter Queue.
* Hoàn thiện demo tạo đơn, hủy đơn, tải cao và dọn dữ liệu thử nghiệm.
* Triển khai và kiểm thử phiên bản mới trên ECS Fargate.
* Đồng bộ source code và chuẩn bị repository sạch dành cho người thực hành.
* Hoàn thiện phần lớn nội dung workshop theo giao diện AWS đã thực hành.
* Cập nhật Proposal, sơ đồ kiến trúc và chi phí vận hành.
* Hoàn thiện báo cáo sự kiện, Self-assessment và Sharing & Feedback.


