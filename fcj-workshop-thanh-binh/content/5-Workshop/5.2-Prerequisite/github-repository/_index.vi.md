---
title: "Repository thực hành"
date: 2026-11-20
weight: 1
chapter: false
---

Source chính của workshop:

```text
https://github.com/sneakez/nexus-aws
```

## 1. Clone source để triển khai thủ công

Mở PowerShell tại thư mục muốn lưu project:

```powershell
git clone https://github.com/sneakez/nexus-aws.git
Set-Location nexus-aws
git status
git branch --show-current
git log -1 --oneline
git rev-parse HEAD
Test-Path backend/Dockerfile
Test-Path supabase/seed.sql
Test-Path .github/workflows/deploy-aws.yml
```

Kết quả cần đạt:

```text
Branch: main
Working tree: clean
Ba lệnh Test-Path: True
```

Ghi lại commit SHA từ `git rev-parse HEAD`. Thực hiện toàn bộ workshop trên cùng commit này.

## 2. Dùng source đã có

Mở PowerShell tại thư mục gốc của repository:

```powershell
git status
git remote -v
git branch --show-current
git fetch --all --prune
git log -1 --oneline
git rev-parse HEAD
Test-Path backend/Dockerfile
Test-Path supabase/seed.sql
Test-Path .github/workflows/deploy-aws.yml
```

Kiểm tra một remote trỏ đến `sneakez/nexus-aws`. Ba lệnh `Test-Path` phải trả về `True`.

Nếu source đang có thay đổi chưa commit, lưu phần đang làm trước khi pull hoặc đổi branch.

## 3. Fork repository để thực hành CI/CD

Phần GitHub Actions yêu cầu quyền mở **Settings**, tạo secrets và variables. Mở repository chính rồi chọn **Fork**:

```text
https://github.com/sneakez/nexus-aws
```

Sau khi fork, clone repository thuộc tài khoản của bạn:

```powershell
git clone https://github.com/<GITHUB_OWNER>/nexus-aws.git nexusoms-workshop
Set-Location nexusoms-workshop
git remote add upstream https://github.com/sneakez/nexus-aws.git
git remote -v
git status
```

Kết quả cần đạt:

```text
origin: repository fork của bạn
upstream: https://github.com/sneakez/nexus-aws.git
```
