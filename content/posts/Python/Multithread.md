---
title: Airflow Introduction
tags:
  - python
categories:
  - Develop
date: 2025-04-21
draft: true
---

python 限制
GIL（Global Interpreter Lock）:目的爲保護數據的安全性。在某個線程中要執行前，需要先拿到GIL，否則無法執行。

基本程式碼
threading.active_count() 用來查看目前有多少個線程
threading.enumerate() 目前使用線程的資訊
threading.current_thread().name 可以用來查看你在哪一個執行緒當中
threading.Thread(target=function,name='線程名字',args=參數) 用來指定你要執行哪一個function，target那邊打上你要的function，你也可以幫線程取名子，在name那邊打上即可，如果要加入參數，在args那邊打上就好
.start()開始執行你所指定的線程，在.前面打上你線程的名字
.join()當你接下來的要執行程式需要你子程序的資料時可以使用這個來讓程式進入暫停，直到子程序執行完畢

單執行緒
多執行緒
守護線程
Queue
lock
semaphore
rlock

參考資料 
[1](https://medium.com/jeasee%E9%9A%A8%E7%AD%86/python-%E5%A4%9A%E7%B7%9A%E7%A8%8B-eb36272e604b)
[2](https://blog.gtwang.org/programming/python-threading-multithreaded-programming-tutorial/)

若遇到 GIL 的情況，可考慮使用多進程

https://ycc.idv.tw/multithread-multiprocess-gil.html