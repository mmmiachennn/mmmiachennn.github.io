---
title: ClickHouse
tags:
  - Cache
categories:
  - Develop
date: 2025-04-21
draft: true
---

producer/consumer

topic:訊息分類（類似資料）
producer：將訊息寫入 topic
consumer：從 topic 讀取訊息（Consumer Group 可以是群集）


類似 pubsub


broker：cluster機器（存放 topic 資料的地方）
partition：資料 partition （理想上有幾個 borker 一個 topic 就會切幾個 partition）
offset：partition 中賦予 event 的 ID，在同一個 partition 中保證數據順序性。同時也是記錄上次抓取到的資料的標籤。



consumer Group 與 Topic partition 的關係：https://ithelp.ithome.com.tw/articles/10360261


監控指標
cmak 監控 UI。
upderReplicated:（有未同步副本） broker 有問題
吞吐量：
負載均衡： skewed

comsumer_offset  檢查資料是否平衡


設定同一個會員寫入同一個 partition 的話，需要將該值設定爲 key。value 則爲資料內容。


# spark structured streaming
兩個 spark straeming 不同
https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html
差別 sss -> sparkSQL ss -> streamingContext


continuous，micro batch
epoch maker

window operation on event time
使用資料本身時間戳處理，並不是資料接收時間處理

watermarking （dataflow 也有）

# kafka 串 sss

spark-sql-kafka

outputmode complete 新舊資料合併計算
 append 只計算新資料
 


延伸議題
- join operation
- monitoring streaming queries
- performance tuning

# google pubsub vs kafka

https://www.linkedin.com/pulse/apache-kafka-vs-google-cloud-pubsub-which-messaging-system-de-luca

