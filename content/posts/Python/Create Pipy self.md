---
title: Airflow Introduction
tags:
  - python
categories:
  - Develop
date: 2025-04-21
draft: true
---

[參考](https://blog.csdn.net/l782060902/article/details/124951775)

- 下載所需要的 package
pip download fastapi -d package
pip download -r requirements.txt -d package

- 啟動服務
http.server 是 python 自帶模組，可用過 python -m 直接使用
python -m http.server 80 -d package

- 使用教程
pip install fastapi --no-index -f http://127.0.0.1/

2.0 pypiserver
安裝、啟動服務
pip install pypiserver
pypi-server run -p 8080 ./package

使用教程
pip install fastapi -i http://127.0.0.1:8080/simple/ some-package