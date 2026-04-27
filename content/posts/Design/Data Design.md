---
title: ClickHouse
tags:
  - OLAP
categories:
  - Develop
date: 2025-04-21
draft: true
---

## 數據密集型

- database：儲存資料、可讀取資料
- caches：運算複雜度高的cache
- search indexes：關鍵字搜尋
- stream processing：即時分析
- batch processing：batch 分析


## 計算密集型


## 可靠性
即使出現錯誤也能繼續正常工作
1. 能預料且應對故障的系統稱爲容錯（fault-tolerant）或韌性（resilient）
2. 


# lambda 結構
1， 流處理器消耗事件並快速生成對視圖的近似更新
2， 批處理器稍後將使用同一組事件並衍生視圖的更正版本

# 大架構資訊

https://akuma1.pixnet.net/blog/post/170528855


# Define DATA
 所有資料只要沒有可以辨別的 id 或 複合 id 一律爲不合格資料
 1. 無法驗證資料的正確性
 2. 處理資料也有困難
 3. 
 
 