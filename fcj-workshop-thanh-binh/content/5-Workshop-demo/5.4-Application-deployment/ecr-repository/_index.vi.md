---
title: "ECR repository"
date: 2026-11-13
weight: 1
chapter: false
---

Tạo private repository, bật immutable tag và scan on push. Luôn copy URI từ Console thay vì tự gõ.

## 8.1. Tạo ECR repository theo giao diện hiện tại

1\. Chọn region ap-southeast-1.

2\. Mở **Amazon ECR → Private registry → Repositories → Create repository**.

3\. Visibility: **Private**. Repository name: nexusoms-demo-api.

4\. Image tag mutability: **Immutable**.

5\. Encryption: giữ AES-256 mặc định.

6\. Phần **Image scanning settings - deprecated** có thể chỉ còn toggle **Scan on push**. Đây là cấu hình cũ đã được chuyển lên registry-level; không cần tìm menu Basic scanning. Để tạo repository có thể giữ toggle theo lựa chọn demo của bạn; việc scanning không quyết định task có pull image được hay không. Nếu cần bật scan, bật Scan on push hoặc cấu hình trong **ECR → Private registry → Features & settings** sau này.

7\. Tạo repository. Mở repository và dùng **Copy URI**. URI phải kết thúc bằng /nexusoms-demo-api; lưu lại nguyên văn.

Đừng tự gõ account ID trong URI nếu có thể copy từ Console. Không dùng latest: repository immutable từ chối ghi đè tag đã tồn tại.

Ảnh cần chụp: 06-ecr/image-push/repository-created.png và 06-ecr/image-push/image-pushed.png.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**Private**

```text
nexusoms-demo-api
```

**Private subnet value**

```text
Private
```

**Private**

```text
Immutable
```

**Scan on push**

```text
Scan on push: Enabled
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open ECR dashboard and create repository

Trên AWS Console, thực hiện **Open ECR dashboard and create repository** như hình dưới đây.


{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/1-open-ecr-dashboard-and-create-repository.png" title="Open ECR dashboard and create repository" >}}

### Bước 2 - Configure private repository

Trên AWS Console, thực hiện **Configure private repository** như hình dưới đây.

**Giá trị dùng ở bước này**

**Private**

```text
nexusoms-demo-api
```

**Private subnet value**

```text
Private
```

**Private**

```text
Immutable
```

**Scan on push**

```text
Scan on push: Enabled
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/2-configure-private-repository.png" title="Configure private repository" >}}

### Bước 3 - Confirm encryption and create repository

Trên AWS Console, thực hiện **Confirm encryption and create repository** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/3-confirm-encryption-and-create-repository.png" title="Confirm encryption and create repository" >}}

### Bước 4 - Copy ECR repository URI

Trên AWS Console, thực hiện **Copy ECR repository URI** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/4-copy-ecr-repository-uri.png" title="Copy ECR repository URI" >}}
