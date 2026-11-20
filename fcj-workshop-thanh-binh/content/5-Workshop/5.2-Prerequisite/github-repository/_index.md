---
title: "Workshop repository"
date: 2026-11-20
weight: 1
chapter: false
---

The workshop source repository is:

```text
https://github.com/sneakez/nexus-aws
```

## 1. Clone the source for manual deployment

Open PowerShell in the directory where the project will be stored:

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

Expected result:

```text
Branch: main
Working tree: clean
All three Test-Path commands: True
```

Record the commit SHA returned by `git rev-parse HEAD`. Use the same commit for the complete workshop.

## 2. Use an existing checkout

Open PowerShell at the repository root:

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

Confirm that one remote points to `sneakez/nexus-aws`. All three `Test-Path` commands must return `True`.

Save any uncommitted work before pulling or switching branches.

## 3. Fork the repository for CI/CD

GitHub Actions requires access to **Settings**, secrets, and variables. Open the source repository and choose **Fork**:

```text
https://github.com/sneakez/nexus-aws
```

Clone your fork:

```powershell
git clone https://github.com/<GITHUB_OWNER>/nexus-aws.git nexusoms-workshop
Set-Location nexusoms-workshop
git remote add upstream https://github.com/sneakez/nexus-aws.git
git remote -v
git status
```

Expected result:

```text
origin: your fork
upstream: https://github.com/sneakez/nexus-aws.git
```
