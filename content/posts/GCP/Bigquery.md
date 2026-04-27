---
title: Apache Livy
tags:
  - Spark
categories:
  - Develop
date: 2025-04-21
draft: true
---

## Partitioned Table

將資料分割成不同區塊，可增進 Query 效能，減少 Query 讀取的資料量。
在 BQ 中可根據一下資料欄位進行 partition
1. 時間欄位
2. 資料匯入時間
3. 整數欄位


## Clustered Table

根據一個或多個指定欄位的值來進行重新排序，可提升 Query 效能，減少 Query 成本花費。Query 語法中過濾條件、聚合運算包含可指定欄位時，可避免掃描不必要的數據。若使用多個指定欄位來創建分群資料表時指定欄位的順序決定了數據的排序順序，因此必須特別注意指定欄位的順序。若有新的資料寫入 Clustered Table， BigQuery 將會自動根據其指定欄位的值進行重新排序 (Auto re-clustering)。

此外，若查詢一個小於 1GB 資料表或分區時，Clustered Table 在效能上並不會有太顯著的進步。

Mia： 相當於 DB 的 index

參考資料：[1](https://hkmci.com/zh-hant/%E7%B6%B2%E8%AA%8C/bigquery-%E6%9C%80%E4%BD%B3%E5%8C%96%E5%AE%8C%E7%BE%8E%E5%AF%A6%E8%B8%90partitioned-and-clustered-table/)


group by cube ()
group by rollup ()
group by rollup (())

string_agg 



## copy table

create table new_table like original_table;
insert into new_table select * from original_table;

create table table_name;
drop table table_name;
alter table original_table rename to new_table;

好的管理 DB 方式，讀寫分離，可以減輕DB 的負載，同時也可以當有活動的時候，只要升規讀的機器即可。

寫入 DB 要分 batch，對 DB 負載較輕，但同時會拉長寫入時間，需要做衡量。

# Bigquery
## select multiple table
https://cloud.google.com/bigquery/docs/querying-wildcard-tables?hl=zh-cn
