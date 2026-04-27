---
title: Git Command
tags:
  - git
categories:
  - Common Command
date: 2026-04-21
draft: true
---


[Git 工具書](https://git-scm.com/book/zh/v2)

### rename branch
```
# 不在 old_branch_name 分支上
> git branch -m old_branch_name new_branch_name

# 在 old_branch_name 分支上
> git branch -m new_branch_name
```

### 刪除 branch
```
> git branch -d branch_name # merge 完之後可以刪除
> git branch -D branch_name # 強制刪除
```

### 查看分支狀況
```
> git branch
```

### 合併 branch
- 合併 A 分支到 B 分支(當前目錄在 B 分支下，要將 A 分支合併到 B 裡)
```
> git merge A
```

### git stash
```
# 暫存檔案列表
> git stash list

# 讀取暫存檔案
> git stash pop
> git stash pop stash@{number} 

# 刪除暫存檔案
> git stash drop
> git stash drop stash@{number} 
```

### 建立 ssh 金鑰

```
# git config 全域
git config --global user.name MiaChen
git config --global user.email "abcd@email.com"
git config --global --list

# create ssh key, 有 rsa、ed25519 兩種
ssh-keygen -t ed25519 -C "你的 git 登入信箱"
```

[github 金鑰教學](https://blog.jaycetyle.com/2018/02/github-ssh/)