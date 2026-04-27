---
title: Apache Livy
tags:
  - Spark
categories:
  - Develop
date: 2025-04-21
draft: true
---



## 在 class 中，需要被 udf 使用的 function 必定要裝上
@staticmethod
疑問 @classmethod 可以嗎？？？


https://liketea.xyz/Spark/Spark/Spark%20%E6%8C%87%E5%8D%97%EF%BC%9ASpark%20SQL%EF%BC%88%E4%BA%8C%EF%BC%89%E2%80%94%E2%80%94%20%E7%BB%93%E6%9E%84%E5%8C%96%E6%93%8D%E4%BD%9C/


Out Of Memory 問題

分為兩種，物理性資源不足以及資料量太大，task並行度太低，導致資源過度集中在一台機器身上