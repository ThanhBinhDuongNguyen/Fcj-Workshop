---
title: "Blog 1"
date: 2026-11-24
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# GITHUB ACTIONS DEPLOY LÊN AWS KHÔNG DÙNG ACCESS KEY: OIDC HOẠT ĐỘNG NHƯ THẾ NÀO?

### Giới thiệu

Trong NexusOMS, nhóm dùng GitHub Actions để triển khai backend lên ECR/ECS và frontend lên S3/CloudFront. Bài viết giải thích cách kết hợp OpenID Connect (OIDC) với AWS STS để workflow nhận credential tạm thời, thay cho việc lưu access key dài hạn trong GitHub Secrets.

### Nội dung chính

1. **Yêu cầu token:** Workflow lấy OIDC token từ GitHub, chứa thông tin về danh tính của job cùng các claim như aud và sub.
2. **Assume role:** Workflow gửi token tới AWS STS qua AssumeRoleWithWebIdentity.
3. **Kiểm tra điều kiện:** AWS kiểm tra token và trust policy của IAM role. Điều kiện phải giới hạn đúng repository, branch và audience theo cấu hình OIDC thực tế.
4. **Cấp credential:** Khi hợp lệ, STS trả credential tạm thời để job gọi AWS API.
5. **Giới hạn thao tác:** Permission policy của role quy định các quyền triển khai đối với ECR, ECS, S3 và CloudFront.

### Bài học rút ra

Trust policy quyết định workflow nào được assume role; permission policy quyết định role được làm gì. Cần kiểm tra cả hai phần khi pipeline bị AccessDenied, thay vì tạo access key dài hạn hoặc mở rộng quyền để thử cho chạy.

### Nguồn tham khảo

- [OpenID Connect trong GitHub Actions](https://docs.github.com/en/actions/concepts/security/openid-connect)
- [Cấu hình OIDC với AWS](https://docs.github.com/en/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-aws)

### Bài viết và minh chứng

Bài viết đã được duyệt và đăng trên AWS Study Group.

[Xem bài viết trên Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2346635502768075)

![Minh chứng bài Blog 1 đã đăng trên AWS Study Group](/images/3-Blog/blog1.png)
