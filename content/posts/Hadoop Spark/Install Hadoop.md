---
title: Apache Livy
tags:
  - Hadoop
categories:
  - Develop
date: 2025-04-21
draft: true
---

## 參考資料

hadoop install

https://data-flair.training/blogs/install-hadoop-on-ubuntu/

50070 Port 無法開啟
https://blog.csdn.net/qq_42992973/article/details/113831751

hadoop簡易架設不求人
https://www.cc.ntu.edu.tw/chinese/epaper/0036/20160321_3609.html

## hadoop + yarn + spark 簡單架構

master*1、worker*5
namenode*2： active、standby
journalnode*3
resource manager*2: 其中有一台為 jobHistoryServer
zookeeper*3
zkfc *2
datanode*5
nodemanager*6
mount NFS ?? 不知道是什麼意思

## 重要但目前無法歸類

- RAID：高可用的工具，有很多不同的做法，來防止資料遺失或是增加效率。在 hadoop 中，NameNode 可以用是因爲 namenode 只有 secondary，需要做HA 的機制；然而，DataNode不能用是因爲 hadoop 本身就有做 replica 的機制來防止資料遺失，不需要重複做這個工作。
- Namenode： keeps metadata in memory。
 

## Hive, Hbase, Impala

- Hive: OLAP，不適合一直更動資料，但很適合拿來做分析。
- Impala: OLTP，更動較快速。
- Hbase: 半結構化資料庫，適合存稀疏矩陣的資料。類似 mongoDB 。

- Kudu：
- HDFS：


## 網路設定

- Rack Awareness：Data high availability and reliability、 Performance of the cluster、 Network bandwidth

## CDH 與 hadoop 相關的 service (沒有很重要，很多都有替代工具了)


- oozie：排程管理系統。替代工具爲 airflow。
- sqoop：搬 RDBMS 的資料，以 batch 的方式進到 HDFS，再轉換成 hive 或 hbase。替代工具爲 DataX、Flink CDC、Debezium、AWS Glue、GCP dataflow。
