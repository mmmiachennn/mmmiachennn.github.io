---
title: ClickHouse
tags:
  - OLTP
categories:
  - Develop
date: 2025-04-21
draft: true
---

## 概念

### 建立 partition
不能建立太多，太多效能反而會差 [測試](https://ithelp.ithome.com.tw/m/articles/10231037)
要建立對的欄位
oracel 有 local index 、global index

[其他參考](https://kkc.github.io/2017/07/07/mysql-partitioning/)

### 建立 index

短索引（前綴索引）
更新頻繁的、數據區分度不高的列不宜建立
單表索引建議控制在 5 個以內。
根據使用需求


### 讀寫分離

好的管理 DB 方式，讀寫分離，可以減輕 DB 的負載，同時也可以當有活動的時候，只要升規讀的機器即可。
寫入 DB 要分 batch，對 DB 負載較輕，但同時會拉長寫入時間，需要做衡量。




PL/SQL
https://www.jendow.com.tw/wiki/PLSQL


## MySQL

### 進入 database
```
use database

show database;

show tables;

select table_schema, table_name, index_name, column_name from information_schema.statistics where table_schema = "yout database";
```

### Event Scheduler

```
SELECT * FROM information_schema.partitions WHERE TABLE_SCHEMA='mia_test' AND TABLE_NAME = 'parti_scheduler_test' AND PARTITION_NAME IS NOT NULL;

show events;

drop event event_name;
```

### About partitions
- 注意： partition 如果是日期的話只能建立比原本 partition 大的日期，無法往前建立

### 建立新的 partition
```
alter table table_name add partition (partition partition_name values less than(condition));

example 
alter table parti_scheduler_test add partition (partition p20240102 values less than(to_days('2024-01-03')));
```

### 刪除 partition
```
alter table table_name drop partition partition_name;
```

### 建立 partition 排程

http://mysql.taobao.org/monthly/2023/02/02/
https://help.aliyun.com/zh/polardb/polardb-for-mysql/user-guide/automated-management-of-partitions


### 參考資料

https://chartio.com/resources/tutorials/how-to-grant-all-privileges-on-a-database-in-mysql/



## Postgres SQL

### Install on Ubuntu

https://www.digitalocean.com/community/tutorials/how-to-install-postgresql-on-ubuntu-20-04-quickstart

https://www.startdataengineering.com/post/data-engineering-project-for-beginners-batch-edition/?fbclid=IwAR07y1C1zwbOb4CMS0cBFX0FaC-ww1o5YZTKQ2GS01RFeNuYuvvEAecLQEE


