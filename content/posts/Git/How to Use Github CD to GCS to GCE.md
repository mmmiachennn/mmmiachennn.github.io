---
title: How to Use Github CD to GCS to GCE
tags:
  - Git
categories:
  - Develop
date: 2026-04-21
draft: true
---

## 環境介紹

GCE VM 建立在內網且無法連線至外網，要把位於外網 github 的程式碼異動自動化部屬到 VM 上。基於安全考量，較好的做法應該是在內網架 git 直接 CD 部屬到 VM。
本篇不探討為何要這樣做，只是提供一個做法參考並紀錄。


## GCE VM 的準備

- 確認 gcloud SDK 是否有安裝

- 安裝 pyenv
    - 有線安裝，pyenv install 3.X.X
    - 離線安裝
        - download Python3.X.X.tar.xz。(https://www.python.org/ftp/python/3.X.X/Python-3.X.X.tar.xz)
        - 將下載的檔案放入 .pyenv/cache
        - 執行 pyenv install 3.X.X 
        - pyenv versions 確認安裝成功

- 安裝 cloud storage fuse
    - sudo apt-get install fuse gcsfuse
    - gcsfuse -v

- 設定環境變數 (Optional)
    - GOOGLE_APPLICATION_CREDENTIALS=/path/service_account/xxx.json

- gcs mount vm 
    - folder: gcsfuse --only-dir --implicit-dirs bucket/path bucket_name /vm/path/
    - mount all bucket: gcsfuse --implicit-dirs bucket_name /vm/path/

- 設定開機自動重跑 mount

- 其他參數設定 [GCS 文件](https://cloud.google.com/storage/docs/cloud-storage-fuse/cli-options)

- --o 參數設定 [Linux fuse 文件](https://man7.org/linux/man-pages/man8/mount.fuse3.8.html)


## Github actions 準備

- 儲存 service_account 在 github 的 setting > secrets and variables 

- yaml
```
name: Repo Code To GCS

on:
  push:
    branches: ["feature/**"]

jobs:
  job_id:
    runs-on: ubuntu-latest
    steps:
    - id: checkout
      uses: actions/checkout@v3

    - id: auth
      uses: google-github-actions/auth@v1
      with:
        credentials_json: ${{ secrets.SERVICE_ACCOUNT }}

    - name: Set up Google Cloud SDK
      uses: google-github-actions/setup-gcloud@v1
      with:
        version: >= 363.0.0

    - name: Upload code to GCS
      run: gsutil -m rsync -d -r action/path gs://bucket-name/path

```

## 結語

只要 github actions 的腳本寫好，CD 啟動程式 rsync 到 bucket，由於 bucket 與 VM 有做 mount，所以會自動同步到 VM 上。這樣就達成了自動部署流程。


## FAQ

1. gsutil "cannot open path of the current working directory: Permission denied". 解決方法：調整 --file-mode 或 --dir-mode 的設定，查詢 linux fuse 相關設定。

2. --implicit-dirs 參數：若沒有加入此參數，使用 gsutil 做的任何變動，將不會被 VM 的資料夾同步。
