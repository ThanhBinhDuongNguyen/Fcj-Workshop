---
title: "Worklog Tuần 11"
date: 2026-11-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Hoàn thiện website báo cáo và chuẩn bị source để các thành viên cập nhật phần cá nhân.
* Điều chỉnh kiến trúc, Proposal và tài liệu triển khai theo góp ý của nhóm.
* Hoàn thiện ba bài blog đã được duyệt trên AWS Study Group.
* Kiểm tra và sửa quy trình triển khai website bằng GitHub Pages.

Nội dung được chốt đến ngày 27/11/2026. Các công việc đầu tuần được tổng hợp chung cho ngày 23–24/11.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2–3 | - Rà soát cấu trúc website, nội dung Workshop, liên kết, hình ảnh và các khối lệnh có thể sao chép. <br> - Kiểm tra Hugo build và Git diff trước khi chia sẻ source cho nhóm. <br> - Xóa Worklog Week 12 theo thời hạn nộp bài; điều chỉnh mức tự đánh giá về kỷ luật và hợp tác nhóm. <br> - Cập nhật thời gian thực tập thành 14/09/2026–30/11/2026 và bổ sung lớp LavieTeam. <br> - Cập nhật Blog 1 về GitHub Actions OIDC và Blog 2 về Task Role/Execution Role bằng nội dung tóm tắt song ngữ, nguồn tham khảo và đường dẫn bài đã được duyệt. | 23/11/2026 | 24/11/2026 | [Workshop](/vi/5-workshop/) <br> [Các bài blog](/vi/3-blogsposted/) |
| 4 | - Đối chiếu góp ý kiến trúc với source backend và tài liệu AWS. <br> - Xác nhận API và background worker chạy chung trong mỗi Fargate task. <br> - Hoàn thiện sơ đồ: phân biệt luồng gửi/nhận message SQS, bổ sung S3 lưu ECR image layers và giữ đầy đủ các bước CI/CD. <br> - Làm rõ tích hợp sàn là kiến trúc mục tiêu, còn demo sử dụng webhook simulator. <br> - Đồng bộ giải thích trong Proposal và Workshop ở hai ngôn ngữ; cập nhật Project Summary. <br> - Phân biệt cấu hình demo một task với cấu hình hai task trên hai AZ; giữ số liệu dự toán và ghi rõ giả định sử dụng. <br> - Bổ sung phạm vi mô phỏng vào mô tả video demo. | 25/11/2026 | 25/11/2026 | [Proposal](/vi/2-proposal/) <br> [Tổng quan Workshop](/vi/5-workshop/5.1-workshop-overview/) |
| 5 | - Cập nhật liên kết menu AWS Study Group - Blog sang https://cloudjourney.awsfcaj.com/ cho cả tiếng Việt và tiếng Anh. <br> - Kiểm tra Git diff sau thay đổi. | 26/11/2026 | 26/11/2026 | [First Cloud Journey](https://cloudjourney.awsfcaj.com/) |
| 6 | - Chẩn đoán lỗi GitHub Actions không tìm thấy cấu hình Hugo do chạy sai thư mục. <br> - Sửa đường dẫn source và output artifact, đồng bộ phiên bản Hugo với local, xử lý URL ảnh/CSS dưới /Fcj-Workshop/ và vô hiệu hóa workflow Jekyll trùng chức năng. <br> - Hoàn thiện Blog 3 về triển khai .NET REST API trên ECS Fargate, bổ sung link Facebook và ảnh minh chứng. <br> - Chuẩn hóa tiêu đề trang/menu thành Blog 1–3 và Event 1–4. <br> - Đưa ba blog về cùng cấu trúc song ngữ; kiểm tra liên kết, ảnh, nội dung hiển thị và Hugo build. | 27/11/2026 | 27/11/2026 | [Các bài blog](/vi/3-blogsposted/) <br> [Các sự kiện](/vi/4-eventparticipated/) |

### Kết quả đạt được tuần 11:

* Thống nhất tài liệu kiến trúc, Proposal, Workshop và Project Summary về phạm vi triển khai và demo.
* Hoàn thiện nội dung báo cáo cho ba bài blog đã được duyệt trên AWS Study Group.
* Chuẩn hóa cách trình bày Blogs và Events theo template.
* Sửa nguyên nhân lỗi build Hugo trong workflow GitHub Pages; build thử local thành công và xác nhận đường dẫn ảnh/CSS phù hợp.
* Kiểm tra bản build gồm 140 trang tiếng Anh và 139 trang tiếng Việt; Git diff không có lỗi.
* Chuẩn bị source để nhóm tiếp tục cập nhật phần cá nhân và hoàn thiện hồ sơ nộp bài.


