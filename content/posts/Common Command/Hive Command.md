---
title: Hive Command
tags:
  - hadoop
categories:
  - Common Command
date: 2026-04-21
draft: true
---

## CREATE

### copy table schema includ partition

```
CREATE TABLE IF NOT EXISTS db.tableA LIKE tableB;
```

### create new table

```
-- has data
CREATE TABLE db.tablA as
SELECT * FROM db.tableB 
where col_a = '12';

-- only schema
CREATE TABLE IF NOT EXISTS db.tableA (
    col_a STRING COMMENT 'a 文字',
    col_b INTEGER COMMENT 'b 數字',
) COMMENT 'mia 測試報表'
PARTITIONED BY (col_c INT)
STORED AS ORC --PARQUET 也可以吧
;
```

## Insert

### insert data to table which has partition 

```
-- 啟用動態分區 (通常在實際執行前會設定)
SET hive.exec.dynamic.partition = true;
SET hive.exec.dynamic.partition.mode = nonstrict;

-- 將資料從 tableA 插入到 tableB
INSERT INTO TABLE db.tableA partition(col_a, col_b)
SELECT * FROM db.tableB 
WHERE col_a = 'N' and col_b = 'cc' and col_c = 123;

-- 寫入新資料
INSERT INTO TABLE db.tableA partition(col_a)
(col_a, col_b, col_c) 
VALUES                        
('123', 334, 1),
('923', 934, 0)
;

-- 危險，小心使用
-- 將資料從 tableA 插入到 tableB
INSERT OVERWRITE TABLE db.tableA PARTITION (col_a) 
SELECT *
FROM db.tableB
where col_b >= 20251101 
;

```

### no partition

```
INSERT INTO db.tableA AS 
SELECT *
FROM db.tableB
WHERE col_a = '12'
;
```


## DELETE

### drop table

```DROP TABLE IF EXISTS db.tableA;```

### drop specify record

```DELETE FROM db.tableA WHERE col_a = '';```

### drop specify partition

```ALTER TABLE db.table DROP IF EXISTS PARTITION (col_a = 'N', col_b = 'cc');```


## ALTER

### add new column

```
ALTER TABLE db.tableA ADD COLUMNS (
    col_z STRING COMMENT 'z 軸',
    col_y STRING COMMENT 'y 軸',
    col_x STRING COMMENT 'x 軸',
);
```

#### rename column

```
ALTER TABLE  db.tableA CHANGE col_old col_new string;
```

## hive 其他設定參數（待研究）
set hive.exec.dynamic.partition=true;
set hive.exec.dynamic.partition.mode=nostrick;
set mapreduce.map.memory.mb=10240;
set hive.exec.max.dynamic.partitions=20000;
set hive.exec.max.dynamic.partitions.pernode=20000;
set hive.exec.max.created.files=20000;


## 進階語法
```
WITH 
  cleaning as (
      select col_a, col_b
      from db.tableA
      where ...
  ),
  spec as (
      select col_a, col_z
      from db.tablB
      where ...
  )

SELECT cleaning.col_a, cleaning.col_b, spce.col_z
from cleaning inner join spec on cleaning.col_a = spec.col_a
where cleaning.col_a = 'Cloud'
;
```