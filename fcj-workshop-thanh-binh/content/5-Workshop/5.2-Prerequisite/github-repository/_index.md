---
title: "Workshop repository"
date: 2026-11-15
weight: 1
chapter: false
---

## 1. Clone the source

```powershell
git clone https://github.com/ThanhBinhDuongNguyen/nexusoms-workshop.git
cd nexusoms-workshop
git status
```

This repository contains only the source, migrations, build configuration, and workflows required by the lab.

## 2. Create a separate repository

Open GitHub → **New repository**.

![Create a repository](/images/5-Workshop/nexusoms/5.2-Prerequisite/github-repository/1-create-github-repository.png)

Use the following name. Do not create a README, `.gitignore`, or license.

```text
nexusoms-workshop
```

![Configure the repository](/images/5-Workshop/nexusoms/5.2-Prerequisite/github-repository/2-configure-github-repository.png)

Copy the repository URL.


```powershell
git remote rename origin upstream
git remote add origin https://github.com/<GITHUB_OWNER>/nexusoms-workshop.git
git push -u origin main
git remote -v
git branch --show-current
```
