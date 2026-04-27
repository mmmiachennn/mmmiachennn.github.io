---
title: Spark
tags:
  - Spark
  - pyspark
categories:
  - 開發相關
date: 2021-12-20
draft: true
---

## Spark 簡介
使用於大規模數據的處理引擎，可用 scala, jave, python, R 來撰寫。
與 MapReduce不同的地方在於，Spark 可以直接在內存中暫存數據，比 MapReduce 把數據寫回磁碟的方式快 100 倍。

- Spark Streaming 流數據處理
- Spark Mlib 機器學習

由於目前工作所需的是 python，所以使用 pyspark 來協助我開發。
在這邊推薦這個網站![Apache Spark Tutorial with Example](https://sparkbyexamples.com/)
對我來說要上手一個工具最快的方法就是從 example 中學習，這樣也比較會有記憶點，
邊做邊學也許會有不同的發想。當初接到的第一個任務就是針對 Spark 效能做調整。

## Spark 效能調較
Spark 有很多參數可以調較，而且也能夠針對機器的規格以及使用情境做不同的調整。舉例來說，如果同時有多個使用者要一起用 spark，
每一個 task 所開放的最大 cpu cores 或是 memory 就不能設定到幾乎跟原始設備一樣的規格，就會有人需要排隊等待，
反之，若是使用人數不多而且每一次 task 跑的數據量較大的話，就需要將設定規格開高一點。
由於 Spark 底層是由 Java 所實現的，因此有很多參數是對 JVM 進行調整，但是對一個剛開始碰 Spark 且只用過 python 的人來說（我）還是先從基礎的功能開始。
Spark 的資料夾中有，spark config 的檔案，不同的 spark mode (ex: Standalone, Cluster, Client)，要設定 config 檔案也不同。

此次分享在 Spark Mesos 的模式下所做的調整，主要是對一個 task 在進行的時候所能使用的最大資源。
- spark.cores.max：給定 spark 可用的核心數量 
- spark.driver.cores：driver 啟動時，可使用的核心數量
- spark.driver.memory：driver 啟動時，可使用的記憶體大小（m, g）
- spark.executor.cores： 一個 executor 可使用的核心數量
- spark.executor.memory：一個 executor 可使用的記憶體大小（m, g）
- spark.rpc.message.maxSize：node 與 node 之間能傳輸的最大資料量（m）


在機器規格固定之下，我只調整以上的 6 個參數，可將原本需要 13 分鐘的演算時間，變成只要 5 分鐘即可跑完。


## Spark + MongoDB
to be contiune。。。。
<!-- 
https://docs.mongodb.com/spark-connector/current/python/write-to-mongodb/ -->



參考資料：
- https://kknews.cc/zh-tw/tech/99xy258.html
