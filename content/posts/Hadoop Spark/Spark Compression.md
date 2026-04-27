---
title: Apache Livy
tags:
  - Spark
categories:
  - Develop
date: 2025-04-21
draft: true
---

- 如何製作 egg file

```
python3 setup.py bdist_egg
```

- 資料分 batch 

https://www.geeksforgeeks.org/pyspark-split-dataframe-into-equal-number-of-rows/

- JDBC
（https://liketea.xyz/Spark/Spark/Spark%20%E6%8C%87%E5%8D%97%EF%BC%9ASpark%20SQL%EF%BC%88%E4%B8%80%EF%BC%89%E2%80%94%E2%80%94%20%E7%BB%93%E6%9E%84%E5%8C%96%E5%AF%B9%E8%B1%A1/）

- dynamic partition overwrite
```
spark = SparkSession \
    .builder \
    .config("spark.sql.sources.partitionOverwriteMode", "dynamic") \
    .getOrCreate()

partition_cols = ["dt"]
partition_key_df = log_df.select(partition_cols).distinct()
result_df = origin_df.join(partition_key_df, on=partition_cols, how="inner")
result_df = result_df.unionByName(log_df)
result_df.write.partitionBy(partition_cols).parquet(path, mode="overwrite")
```

| 更新方式  | 檔案數量 | 時間   |
| --------- | -------- | ------ |
| Overwrite | 1219     | 3m 11s |
| Dynamic   | 1217     | 2m 56s |


spark 為 lazy fashion。意謂著，即使再任何時間定義新的 RDD，只有當他們首次被用於行動操作是才會被真的計算出來。。spark檢查全部的轉換操作後，他可以之處理必要的資料。事實上，對於 first（）的操作。spark之掃描了第一個符合的行就隨即停止動作了，他甚至沒有讀取整個檔案。

若想要再多個行動操作中重複使用一個同RDD。可以透過RDD.persist()要求spark保留該RDD。反之，再預設情況下，每一次對RDD執行一個行動操作，預設是需要重新計算結果。


## 傳遞函數給 spark

若傳遞一個有欄位參考的函式，spark會傳送整個物件到工作節點，而你可能之需要整個物件中的某一個小部分的資訊而已。取而代之的是，只將所需的欄位從你的物件中抽取出來並放入區域變數內進行傳送
example：
  class SearchFunction(object):
      def __init__(self, query):
          self.query = query
      def isMatch(self, s):
          return self.query in s
      def getMatchesMemberReference(self, rdd):
          return rdd.filter(lambda x: self.query in x) # 問題：再 self.query 參考了所有的 self


class WordFunctions(object):
    …
    def getMatchesNoReference(self, rdd):
        query = self.query
        return rdd.filter(lambda x: query in s)


distinct() 的操作成本很高,此操作必須對所有分散的資料進行洗牌(shuffling),以確保每個元素只接收到一次。同理，subtract、intersection 執行效率也會很差。笛卡兒積（a, b 兩群資料所有的排列組合）對大型 RDD 是非常昂貴的操作


## 資料分割
若RDD只被掃描一次，則事先進行切割就沒有效益點。只有在執行鍵導向（ex join）這類資料會重複多次使用的操作時，分割才有意義。
舉例：有一張大量使用者資訊的表(UserId, UserInfo)，需要定期組合每五分鐘發生的事件表(UserId, LinkInfo)，使用 join。會非常沒有效率的原因是因為，不知道資料集內的鍵值是如何被分割的。預設情況止血，每次呼叫資料表都會進行雜湊然後跨網路執行資料洗牌。
如書本第62、63頁的資料。
傳遞多少數值，代表要求的分割數量，有多少平行任務可以被執行；一般來說，這個數值的大小至少要大於叢集核心數量的總和。



|dataproc | apache livy | spark client |
