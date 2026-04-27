---
title: Github Actions
tags:
  - Git
categories:
  - Develop
date: 2026-04-21
draft: true
---

## 什麼是 github actions

是 Github 在 2019 年才推出的 CI/CD 服務。
- CI：Continuous Integration 持續整合
- CD：Continuous Deployment 持續部署
example：把 restore、build、test、publish、deploy 寫成腳本使其可重複使用。
Github 建制一個可以分享 actions 的 [marketplace](https://github.com/marketplace?type=actions)，使不會寫的人也能夠使用。

### CI 項目以 python 爲例：
- python 版本測試
- 安裝套件
- 檢查程式碼規範
- 單元測試
- 單元測試覆蓋率

## github workflow yaml 介紹

[github doc](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)

- on：workflow 觸發條件。example: 當 main 這個 branch 有 push 或 pull_request 的動作。
- jobs：執行工作。
- runs-on：要運行在哪一個平台上。ex：github 託管 runner 或是自架的環境。
- strategy、matrix：矩陣策略。ex 在不同版本的 python 環境下執行。
- fail-fast：在矩陣中的任何作業失敗的情況下，若參數爲 true ，則取消矩陣中所有作業；參數爲 false，則繼續執行剩下的作業。
- steps：執行步驟。
- name：步驟名稱。
- uses：使用相關的 yaml 檔來源。
- with：相關參數。
- run：執行 commands。

```
name: Python package

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      matrix:
        python-version: ["3.9", "3.10", "3.11"]

    steps:
    - uses: actions/checkout@v3
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v3
      with:
        python-version: ${{ matrix.python-version }}
    - name: Install dependencies
      run: |
        if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
    - name: Lint with flake8
      run: |
        # stop the build if there are Python syntax errors or undefined names
        flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
        # exit-zero treats all errors as warnings. The GitHub editor is 127 chars wide
        flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics
    - name: Test with pytest
      run: |
        pytest

```


## 開發流程 大數據版

![[pchome_cicd_flow.png]]
## git flow

使用 category 區分 branch 項目。
category: hotfix, feature, bugfix
優點：流程嚴謹，能很快掌握產品的狀況。ex： 有那些 feature 正在進行中 。
缺點：新手門檻較高，若不熟悉會造成分支大亂。

## github flow

一有更新就會即時發佈。
優點：新手門檻較低，流程簡單易懂
缺點：若需要有版本管理的話，需要再建立一個 production 分支

## githlab flow

## github action yaml 結構

images/pchome_cicd_flow.png
根據不同專案有不同的 流程

``` sst.yml
on:
 push:
    paths: ["projects/sst/**"]
    branches：[feature/**, bugfix/**]
```
當 branch 名稱開頭爲 feature 或 bugfix 中的檔案 projects/sst 有異動時， workflow 會被觸發。


deploy to GCS
- eccronj mount gcs
- github code rsync gcs

## Tips
pull_request @相關人員
protected branch （對 main做保護，避免新人誤觸）
Squash 多个commit（分支合併到 master 時，將多個 commit 合併成一個。前提是這個 branch 只有一個人在使用 ）
記得 merge 到 main 之後要刪除子分支

