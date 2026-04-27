---
title: Apache Livy
tags:
  - Spark
categories:
  - Develop
date: 2025-04-21
draft: true
---

apache livy

http://10.62.166.7:8999

# Interactive session：互動式、共享對話

## 建立新的 session
- POST
- url: ${livy_url}/sessions
- header: X-Request=Test

## 查詢 session 清單與狀態
- GET
- url: ${livy_url}/sessions

## 取得特定 ID session 狀態
- GET
- url: ${livy_url}/sessions/{sessionId}


# Statement: 下運算指令

## 新增運算指令 
- POST
- url: ${livy_url}/sessions/{sessionId}/statements
- header: X-Request=Test1
- body: {"code": "2 + 2"}

## 取得所有運算指令的清單與狀態
- GET
- url: ${livy_url}/sessions/{sessionId}/statements


## 取得特定 session 中的特定 statements 的狀態
- GET
- url: ${livy_url}/sessions/{sessionId}/statements/{statementId}


# Batches: 批次執行 spark 程式碼

## 建立 batch 任務
- POST
- url: ${livy_url}/batches
- header: X-Request=Test
- body: {"file": "hdfs://ha_user_hank/test_spark.py"}

### body 延伸用法：
{
    "file": "hdfs://xxx.py", ---> string，必填，應用程式檔案（如 jar、.py 檔案）所在的路徑
    "className": "xxxClass", ---> string，選填，提交的是 Java/Scala jar 時，用來指定 main class 名稱
    "args": ["--input", "hdfs:///data/input", ...], ---> array of strings，選填。傳遞給應用程式的參數配置
    "jars": ["hdfs://xxx.jars"], ---> array of strings，選填，要包含到 Spark 程式中的額外 jar 檔清單
    "pyFiles": ["hdfs:///apps/myapp.zip"], ---> array of strings，選填，要包含到 Python 程式中使用的其它 .py、zip 或 egg檔清單
    "files": "files": ["hdfs:///apps/config.yaml"], ---> array of strings，選填，執行程式時，額外的檔案（如 config、data）清單
}


## 取得所有 batch 清單與狀態
- GET
- url: ${livy_url}/batches
- header: X-Request=Test

## 取得特定 ID 的 batch 狀態
- GET
- url: ${livy_url}/batches/{id}
- header: X-Request=Test


# Sentry
資料表權限管理 （hive, hub, impala）

ssh 每台 VM 創建 user

hue 創建 user、group （sentry 的管理 UI）


yarn 切 queue 設定