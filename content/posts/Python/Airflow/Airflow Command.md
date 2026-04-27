---
title: Airflow Introduction
tags:
  - python/airflow
categories:
  - Develop
date: 2025-04-21
draft: true
---

## Airflow 指令

delete 某個 run_id , 可用 UI 介面， Dag Runs 找到該 run_id 做刪除

### backfill (補資料)

其中時間依照 airflow 設定的時區

```
airflow dags backfill -s “2024-12-02T03” -e “2024-12-03T12” dag_id
```

### 如果 backfill 的 task 已執行過，想要再重跑一次

1. clear tasks (UI 或 command)，再執行 backfill 的指令

```
airflow tasks clear -s “2024-12-02T03” -e “2024-12-03T12” dag_id
```

2. 如果 airflow dags 設定 “catchup=True”，可以直接執行 backfill 的指令（可能）


## Airflow 安裝

### 有線安裝


### 離線安裝
p.s. 如果直接複製的話會導致 airflow command not found，原因為 python/bin 下面沒有 airflow 執行檔。雖然使用 python -m airflow 還是可以執行，但為求系統穩定，遵循正規途徑

1. download 相關依賴套件

    ```
    # 還是要知道到底少了哪些套件再一個一個加
    pip download apache-airflow==2.9.1 -d airflow-offline
    pip download wheel setuptools methodtools wirerope -d airflow-offline
    ```
    

2. install packages

    ```
    pip install --no-index --find-links=airflow-offline apache-airflow
    ```
    

3. check airflow installed

    ```
    airflow -h
    ```

### 參數設定

- catchup
是否需要重複回填資料，假設 schedule 開始日是 2022-1-1 每天做一次，但任務是2022-2-1才開始，如果catchup = True排程就會從 2022-1-1 開始把這中間漏掉的日子都做完。


- 使用者建立
airflow users create --username admin --password your_password --firstname your_first_name --lastname your_last_name --role Admin --email your_email@some.com
airflow users list



## 參考網站

Install Ariflow
https://medium.com/@cchangleo/airflow-%E6%96%B0%E6%89%8B%E5%BB%BA%E7%BD%AE%E6%B8%AC%E8%A9%A6-part-1-9e0fe4d12e7a
https://towardsdatascience.com/an-introduction-to-apache-airflow-21111bf98c1f
https://insaid.medium.com/setting-up-apache-airflow-in-ubuntu-d0317a59f8a4

About airflow dubug
https://towardsdatascience.com/getting-started-with-airflow-using-docker-cd8b44dbff98



Install docker airflow
https://ithelp.ithome.com.tw/articles/10331507
https://towardsdatascience.com/getting-started-with-airflow-using-docker-cd8b44dbff98
https://github.com/dalvimanasi/Data-Pipe-lining-Tools/blob/master/Airflow/AirflowDemo/Helloworld.py


start airflow docker take volume

```
docker run -d --name aaairflow -p 8080:8080 -v /home/mia/airflow/dags/:/usr/local/airflow/dags puckel/docker-airflow webserver
```

進階功能
https://www.itread01.com/content/1547052678.html
https://tw.coderbridge.com/series/c012cc1c8f9846359bb9b8940d4c10a8/posts/3d1f5ea33f5f4a3bac33f95099e376e8
https://medium.com/datareply/airflow-lesser-known-tips-tricks-and-best-practises-cf4d4a90f8f



context manager

airflow 提供多個 task 使用同一個參數，不需要重複登入


隱藏登入資訊

在使用 airflow 中，常常會使用到 DB 資訊，airflow 有提供 Admin -> connections，不用讓 DB 資訊之類的私密帳號密碼赤裸裸的寫在 code 裡面。




