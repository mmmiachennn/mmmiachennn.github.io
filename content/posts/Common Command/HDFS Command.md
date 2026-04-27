---
title: HDFS Command
tags:
  - hadoop
categories:
  - Common Command
date: 2026-04-21
draft: true
---

## 參考網站

https://ithelp.ithome.com.tw/m/articles/10191116

## folder 處理

```
## list

hadoop fs -ls hdfs_path

# 參數說明：
#  -C: 只顯示資料夾與檔案的路徑，略過其它資訊。
#  -d: 目錄被列為純文件，只顯示該路徑的資訊。
#  -h: 將檔案大小加上單位，便於閱讀（例如，2310會顯示2.3k）。
#  -R: 列出路徑內所有子目錄及檔案。
#  -t: 按照編輯時間排序（最近的排第一個）。
#  -S: 按檔案大小排序。
#  -r: 顛倒排列順序。（例如，搭配-t使用就會將距離最遠的時間排第一個）
#  -u: 使用訪問時間顯示和排序（預設是使用編輯時間）。

## create new folder

hadoop fs -mkdir hdfs_path

# 參數說明：
#  -p: 類似Unix上 mkdir -p的用法，會將<paths>所有的路徑資料夾全部新增出來。


```



## local 至 hdfs

```
hadoop fs -put local_path hdfs_path

# 參數說明：
#  -p : 保留檔案的瀏覽、修改時間、擁有者和權限。
#  -f : 如果儲存的路徑已經存在相同檔名，就複寫該檔案。
#  -l : 強制將檔案的儲存副本(replication)變成1。表示該檔案在HDFS上只有一份replication，非必要請勿使用，會失去HDFS的可靠(reliable)特點。
#  -d : 不建立副檔名為._COPYING_的暫時(temporary)文件。儲存檔案至HDFS過程中會先建立一個副檔名為._COPYING_的暫存檔，當儲存完畢後再刪除，若使用這參數將會跳過這步驟。
```

## hdfs 至 local

```
hadoop fs -get hdfs_path local_path

# 參數說明：
#  -p: 保留檔案的瀏覽和修改時間，擁有者和權限。 （假設權限可以跨越檔案系統複製）
#  -f: 如果儲存的路徑已經存在相同檔名，就複寫該檔案。
#  -ignorecrc: 略過CRC驗證。
#  -crc: 在檔案下載時寫入CRC認證。

```

## hdfs fsck (file system check)

檢查檔案系統的健康狀況、尋找遺失的數據塊 blocks 以及修復損壞的檔案

- hadoop fsck / -files -blocks: list all the blocks corresponding to each file in the hdfs?

 
- hdfs dfsadmin -printTopology：查看集群節點的機架分配情況


## hdfs Safemode

唯讀保護狀態。檔案系統不允許進行任何修改，只能讀取。常見有兩種場景會進入 safemode:
1.正常啓動的過程。
2.異常保護，namenode 發現資料塊的副本數量不足，爲了防止數據損壞擴大，會自動進入安全模式。

```
# 查看狀態
hadoop dfsadmin –safemode get

# 進入安全模式
hdfs dfsadmin -safemode enter

# 強制離開（危險）
hdfs dfsadmin -safemode leave

```