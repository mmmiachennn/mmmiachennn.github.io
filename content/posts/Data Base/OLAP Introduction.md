---
title: ClickHouse
tags:
  - OLAP
categories:
  - Develop
date: 2025-04-21
draft: true
---

[參考資料](https://aws.amazon.com/tw/what-is/olap/)

## 什麼是 OLAP
OLAP(線上分析處理)，從不同角度分析業務資料的軟體技術。例如，零售商會儲存有關產品銷售的所有資訊，也會蒐集客戶採購資料。OLAP結合所有資料集來快速解答問題。

## 有很多種不同類型的 OLAP 
|Item|ClickHouse|Kudu|Hive|RDBMS (e.g., MySQL/Postgres)|
| --- | --- | --- | --- | --- |
|Type|OLAP|OLAP/OLTP Hybrid|Data Warehouse (OLAP)|OLTP (usually)|
|Primary Key|Yes (Required)|Yes (Required)|Optional (Mostly Metadata)|	Yes|
|Secondary Index|Yes (Skipping Indexes)|No|No (Limited/Legacy)|Yes|
|Partitioning|Yes|Yes|Yes|Yes|
| Summary | ClickHouse uses indexes to speed up scans across massive datasets.|Kudu uses the Primary Key to find specific rows quickly in a distributed environment.|Hive uses partitioning (folders in HDFS) as its main way to "index" or narrow down data.|RDBMS uses B-Tree indexes to ensure high-speed lookups for individual transactions.|

## OLAP 架構
- 資料倉儲（raw data）
- ETL 工具（ODS）
- OLAP 伺服器（kylin）
- OLAP 資料庫（hive）
- OLAP 立方體（cube）：多元資訊陣列的模型。OLAP 立方體是固定的，一旦建立模型，就無法變更維度和基礎資料。
- OLAP 分析工具（superset）
- OLAP 使用多維表達式（MDX）來查詢 OLAP 立方體。MDX 是一個查詢，類似於 SQL，它提供一組用於操作資料庫的指令。


## OLAP 的類型
- MOLAP（多維線上分析處理）
- ROLAP（關聯式線上分析處理）
- HOLAP（混合式線上分析處理），結合MOLAP 與 ROLAP，提供兩種架構的最佳效能。

## 什麼是 OLAP 中的資料建模？
資料建模是資料庫中的資料呈現。它將多維資料儲存爲星型或雪花型結構。

#### 星型結構
由一個事實表和多個維度表組成。事實表是包含與業務程序相關是數值的資料表，而維度表則包含描述事實表中每個屬性的值。事實表指的是具有外部索引鍵（也就是與維度表中各個資訊相關聯的唯一識別碼）的維度表。

在星型結構中，事實表會連接到多個維度表，因此資料模型看起來像星形。以下是產品銷售事實表的範例： 
- 產品 ID
- 位置 ID
- 銷售人員 ID
- 銷售金額
產品 ID 指示資料庫系統從產品維度表中擷取資訊，如下所示：
- 產品 ID
- 產品名稱
- 產品類型
- 產品成本
同樣，位置 ID 指向位置維度表，其中可能包含下列項目：
- 位置 ID
- 國家
- 城市
銷售人員表可能如下所示：
- 銷售人員 ID
- 名字
- 姓氏
- 電子郵件

#### 雪花型結構
是星型結構的延伸你。某些維度表可能會產生一個或多個次要維度表。當維度表放在一起時，形狀像雪花一樣。
例如，產品維度表可能包含下列欄位：
- 產品 ID
- 產品名稱
- 產品類型 ID
- 產品成本
產品類型 ID 連接到另一個維度表，如下列範例所示：
- 產品類型 ID
- 類型名稱
- 版本
- 變體