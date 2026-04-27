---
title: Screen Command
tags:
  - linux
categories:
  - Common Command
date: 2026-04-21
draft: true
---

## 開啟 screen
```
screen
```

## 保留目前執行，並退出 screen
```
先按 ctrl + a 
再按 ctrl d
```

## screen list
```
screen -ls
```

## 進入 screen
```
screen -r XXXX
```
## 刪除 screen
```
screen -S screen_name -X quit...
```