---
title: Apache Livy
tags:
  - Spark
categories:
  - Develop
date: 2025-04-21
draft: true
---

https://www.gushiciku.cn/pl/giTE/zh-tw


## config 設定方法

1. 全域設定，在 $SPARK_HOME/conf/spark-defaults.conf 設定

2. 在 spark session create 的時候設定
```
spark = SparkSession.builder \
    .config("spark.sql.sources.partitionOverwriteMode", "dynamic") \
    .getOrCreate()
```

3. 在 spark submit 的時候設定
```
./bin/spark-submit \
  --class <main-class>
  --master <master-url> \
  --deploy-mode <deploy-mode> \
  --spark.driver.memory 4G
  --spark.executor.memory 20G

./bin/spark-submit \
  --class <main-class>
  --master <master-url> \
  --deploy-mode <deploy-mode> \
  --conf spark.driver.memory=4G
  --conf spark.executor.memory=20G
```



## 常用 config
 
```
# ignore info
spark.sparkContext.setLogLevel('ERROR')
```

### DataFrame

| 參數名稱                     | 定義                                                                                                                                           |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| spark.sql.adaptive.enabled   | 自動調整 shuffle partition                                                                                                                     |
| spark.sql.shuffle.partitions | 設定切割資料個數（應該要根據資料的大小而做調整，不應該寫死）(num executors * num cores) p.s. dataframe 做 aggregate、join 的時候切割資料的數量 |
| spark.sql.auto.repartition   | 自動調整 shuffle partition                                                                                                                     |
| spark.default.parallelism    | RDD 平行化處理，對 dataframe 沒有效                                                                                                            |

### Execution Behavior

| 參數名稱                  | 定義                                                                        |
| ------------------------- | --------------------------------------------------------------------------- |
| spark.default.parallelism | RDD 分割最大數量，當用戶未設置時，cluster 自行決定 shuffle 過程中的任務數量 |
| spark.executor.cores      | Mesos 的粗粒度模式下，worker節點的所有可用的 core                           |
|spark.executor.instances | spark app 執行的 executor 數量,目前沒有變化|


### Dynamic Allocation
|參數名稱| 定義|
|---| ---|
|spark.dynamicAllcation.enabled | 動態配置資源|
|spark.shuffle.service.enabled | spark.dynamicAllcation.enabled 開啟時，要同時開啟這個參數。
|spark.dynamicAllocation.minExecutors | 動態配置資源時，最低限度的 executors 數量
|spark.dynamicAllocation.maxExecutors | 動態配置資源時，最高限度的 executors 數量

### Scheduling

|參數名稱| 定義|
|---| ---|
|spark.cores.max | 一個 APP 所能使用的 core 數量，根據使用者人數來做調整，設定越大，一個task所能使用的資源越多，同時使用的人數就會較少。APP 可以使用的最大 CPU 數量|
|spark.speculation | True，如果有一個或多人物在某一階段運行緩慢，他們將重新啟動。|
|spark.speculation.interval | 多久時間去檢查一次任務|

### Networking

|參數名稱| 定義|
|---| ---|
|spark.rpc.message.maxSize | executor 和 driver 之間溝通設定最大溝通資料量|

