---
title: "Repository thực hành"
date: 2026-11-15
weight: 1
chapter: false
---

## 1. Clone source code

Mở PowerShell tại thư mục muốn lưu project.

```powershell
git clone https://github.com/ThanhBinhDuongNguyen/nexus-aws.git
cd nexus-aws
git status
```

## 2. Tạo repository riêng

Mở GitHub → **New repository**.

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/github-repository/1-create-github-repository.png" title="Tạo repository mới" >}}

Nhập tên repository. Không tạo README, `.gitignore` hoặc license.

```text
nexusoms-workshop
```

{{< figure src="/images/5-Workshop/nexusoms/5.2-Prerequisite/github-repository/2-configure-github-repository.png" title="Cấu hình repository" >}}

Sao chép URL của repository vừa tạo.


Đổi remote và push source lên repository mới.

```powershell
git remote rename origin upstream
git remote add origin https://github.com/<GITHUB_OWNER>/nexusoms-workshop.git
git push -u origin main
```

Kiểm tra:

```powershell
git remote -v
git branch --show-current
```

Kết quả cần đạt: remote `origin` trỏ đến repository mới và branch hiện tại là `main`.
