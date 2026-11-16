---
title: "Repository thực hành"
date: 2026-11-15
weight: 1
chapter: false
---

## 1. Clone source code

Mở PowerShell tại thư mục muốn lưu project.

```powershell
git clone https://github.com/ThanhBinhDuongNguyen/nexusoms-workshop.git
cd nexusoms-workshop
git status
```

Repository này chỉ chứa source, migration, cấu hình build và workflow cần cho bài lab.

## 2. Tạo repository riêng

Mở GitHub → **New repository**.

![Tạo repository mới](/images/5-Workshop/nexusoms/5.2-Prerequisite/github-repository/1-create-github-repository.png)

Nhập tên repository. Không tạo README, `.gitignore` hoặc license.

```text
nexusoms-workshop
```

![Cấu hình repository](/images/5-Workshop/nexusoms/5.2-Prerequisite/github-repository/2-configure-github-repository.png)

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
