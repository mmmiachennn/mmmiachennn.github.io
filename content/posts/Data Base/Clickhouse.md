---
title: ClickHouse
tags:
  - OLAP
categories:
  - Develop
date: 2025-04-21
draft: true
---

擅長 append， update 與 delete 不是那麼擅長


## 表引擎的種類
會分成很多種類的表引擎，除了 Distributed 之外，其他都是 local 表引擎。

### local 表引擎(MergeTree 系列)

這些引擎負責數據的物理儲存、排序、壓縮和過濾。它們只管「自己腳下」的那塊硬碟，不具備跨機器溝通的能力。

- MergeTree: pk 可重複，order by 必填
- ReplacingMergeTree: 根據 order by 去除重複，合併去重、分區去重。由於不知道底層什麼時候才會去重，因此如果確保資料的準確性與一致性的話，不建議使用。但可以在 select 之前加強制執行的語法
- SumingMergeTree: 自動聚合
  
### 高可用引擎(Replicated 系列)
它依然屬於 Local 儲存引擎，但具備「多機同步」能力。
- ReplicatedMergeTree: 它會把寫入動作同步給同一個 Shard 的其他 Replica。它不會幫你做「分片（Sharding）」。它只保證這台機器跟它的備援機器的資料是一模一樣的。
    
### Special 引擎
不存數據，像是一個窗口，負責把你的指令轉發到正確的地方。
- Distributed（分散式引擎）：它就像是一個路由器。當你查它時，它去問各個節點上的 Local 表；當你寫它時，它把數據分發給各個 Local 表。
- View / MaterializedView（視圖）：負責邏輯轉換或觸發數據處理。


## 其他

刪除修改等 動作對 clickhouse 來說效能負擔較重，因此可以透過打 tag 的方式過濾 tag 來組成資料最新狀態是比較好的做法。

clickhouse 沒有 master、worker 之分，機器透過 zookeeper 互爲副本

# Distributed vs local 表



