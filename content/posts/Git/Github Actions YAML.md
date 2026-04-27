---
title: Github Actions YAML
tags:
  - Git
categories:
  - Develop
date: 2026-04-21
draft: true
---

- [Github Actions 官方文件](https://docs.github.com/en/actions/quickstart)


## Trigger 的各種方法
```
# 根據制定資料夾是否有變動來觸發
on:
  push:
    paths: "home/webuser/htdocs/projects/PROJECT_NAME/**"

# 根據 branch 的動作來觸發
on:
  pull_request:
    branches: "main" 
  push:
    branches: "features/**"
  pull:
    branches: ["stg", "main"]
    
# 根據 tag 的動作來觸發
on:
  push:
    tags: 'v*.*.*'
```

## 平行執行很多個 job
```
# 針對不同 python 版本執行(會同時執行 3.8 與 3.9)
jobs:
  build:
    runs-on: 'ubuntu-latest'
    strategy:
      fail-fast: false
      matrix:
        python-version: ["3.8", "3.9"]
    ...

    - name: Set up Python ${{ matrix.python-version }}
      uses: 'actions/setup-python@v3'
      with:
        python-version: ${{ matrix.python-version }}

```

## 使用別人的 github actions script
```
# uses: 來源 workflow。with: 代入參數
- name: 'Set up Google Cloud SDK'
  uses: 'google-github-actions/setup-gcloud@v1'
  with:
	version: '>= 363.0.0'
```
