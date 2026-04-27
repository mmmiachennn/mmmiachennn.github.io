---
title: Apache Livy
tags:
  - Hadoop
categories:
  - Develop
date: 2025-04-21
draft: true
---

## 優化 KUDU
查詢 10 億筆（1 Billion）等級的資料並要求在 1 分鐘內回傳結果，這在 Kudu + Impala 的架構下是完全可行的，但前提是必須精準配置 存儲設計（Storage Design） 與 查詢優化（Query Optimization）。以下是達成此目標的核心技術指南：

1. Kudu 存儲層優化：
    - 分區策略（Partitioning）這是影響效能最關鍵的因素。Kudu 不像 HDFS 有 Partition 映射目錄，它依賴 Tablets。
    - 混合分區（Hybrid Partitioning）： 針對大數據量，建議結合 Hash Partitioning 和 Range Partitioning。
    - Hash： 確保數據均勻分佈在所有 Tablet Server 上，並行利用 CPU 資源。
    - Range： 針對時間維度（如 event_time）進行分區。這樣當你查詢「最近一小時」時，Impala 可以直接進行 Partition Pruning（分區裁剪），完全跳過 90% 以上的無效數據。
    - Tablet 數量掌控： 每個 Tablet Server 的硬碟核心建議配置 10-20 個 Tablets。10 億筆資料建議切分為 50-100 個 Tablets 以上，視你的集群規模而定。
 
2. Impala 查詢層優化：資源與計畫Impala 是 MPP 架構，速度取決於它如何並行處理數據。
    - Compute Stats： 執行 COMPUTE STATS <table_name>。這會讓 Impala 知道數據分佈、基數（Cardinality）和大小，從而產生最優的執行計畫（如選擇正確的 Join 方式）。
    - 多線程並行度（MT_DOP）： 對於掃描大量數據的查詢，可以設置 SET MT_DOP=4;（或更高）。這會強制 Impala 在每個節點上開啟多個線程並行處理單個查詢。
    - 避免全表掃描： 確保查詢條件（WHERE clause）命中了 Kudu 的 Primary Key 的前導列。Kudu 在主鍵上具有索引，這能讓掃描從「全量」變成「範圍精確定位」。

3. 資料佈局與編碼（Encoding & Compression）Kudu 是列式存儲，正確的編碼能極大減少 I/O 壓力。
    - 選擇正確的 Encoding：
        - Bitshuffle / Run Length： 用於重複值多的列（如狀態碼、類別）。
        - Delta Encoding： 用於時間戳或遞增 ID。
    - 壓縮（Compression）： 通常建議使用 LZ4。它在壓縮率與解壓縮速度（CPU 消耗）之間取得了最佳平衡，能顯著加快從硬碟讀取 10 億行數據的速度。
    - 
4. 硬件與集群架構SSD 是標配： Kudu 的設計初衷就是為了在 SSD 上發揮性能。如果是傳統 HDD，1 分鐘查詢 10 億筆會非常吃力。
    - 記憶體容量： Impala 需要足夠的內存來緩存元數據和處理中間運算（如 Group By 和 Join）。
    - 網絡帶寬： 確保節點間是 10GbE 以上的網絡，防止數據 Shuffle 時產生瓶頸。
    
5. 常見場景對照表查詢類型優化關鍵點點查詢 (Point Lookup)必須命中 Primary Key，延遲應在毫秒級。
    - 範圍掃描 (Range Scan)依賴 Range Partitioning 與 LZ4 壓縮減少 I/O。
    - 聚合計算 (Aggregation)依賴 COMPUTE STATS 與 Impala 的內存運算。