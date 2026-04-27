---
title: Github Actions
tags:
  - Git
categories:
  - Develop
date: 2026-04-21
draft: true
---

# git 三種模型

1. 分散式貢獻者模型(dispersed)：
    Linux kernel 使用中、使用 diff 互相傳遞差異檔、可以不需要使用版本控制軟體、以完整概念開發。
    - 優點：強制 code review
    - 缺點：分享成果變複雜、貢獻者自行假設版本管理系統
2. 共處一地貢獻者模型(collocated)：
    透過中央程式碼代管系統、上游專案有完整控制權、貢獻者在代管系統建立複本、貢獻者在複本上變更後發請求至上游、Github 爲濫觴
    - 優點：強制 code review
    - 缺點：過多的 commit 造成除錯困難、每個成員都要建立複本、分享成果方式變較複雜。

3. 共享維護模型(shared)
    大家都有寫入權限、成員間要有互信基礎
    - 優點：鼓勵清楚的 master 分支
    - 缺點：鼓勵但不強制 code review、必須給所有人寫入權限。