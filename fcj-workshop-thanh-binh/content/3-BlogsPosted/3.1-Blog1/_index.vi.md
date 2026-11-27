---
title: "Blog 1"
date: 2026-11-24
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# GITHUB ACTIONS DEPLOY LÊN AWS KHÔNG DÙNG ACCESS KEY: OIDC HOẠT ĐỘNG NHƯ THẾ NÀO?

Trong quá trình xây dựng CI/CD cho NexusOMS, nhóm sử dụng GitHub Actions để triển khai backend lên Amazon ECR và Amazon ECS, đồng thời đưa frontend lên Amazon S3 và Amazon CloudFront. Workflow cần quyền gọi AWS API nhưng không lưu AWS access key dài hạn trong GitHub Secrets.

Giải pháp được sử dụng là **OpenID Connect (OIDC)** kết hợp với **AWS Security Token Service (AWS STS)**.

### Luồng xác thực

1. GitHub Actions yêu cầu OIDC token cho workflow đang chạy.
2. Token chứa các claim như `aud`, `sub`, repository và branch.
3. Workflow gọi `AssumeRoleWithWebIdentity` để yêu cầu AWS STS cấp quyền.
4. AWS kiểm tra token dựa trên trust policy của IAM role.
5. Nếu các điều kiện hợp lệ, AWS STS trả về credential tạm thời cho workflow.
6. Workflow sử dụng permission policy của role để thao tác với ECR, ECS, S3 và CloudFront.

### Các điểm cần lưu ý

- Không lưu access key dài hạn trong GitHub Secrets.
- Credential chỉ tồn tại trong thời gian ngắn và được cấp lại cho từng workflow run.
- Trust policy cần giới hạn đúng repository, branch và audience.
- Permission policy chỉ cấp các quyền cần thiết cho quá trình triển khai.
- OIDC xác định workflow nào được phép assume role; permission policy xác định role được phép thao tác với tài nguyên nào.

Bài viết đã được duyệt và đăng trên AWS Study Group:

[Xem bài viết trên Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2346635502768075)
