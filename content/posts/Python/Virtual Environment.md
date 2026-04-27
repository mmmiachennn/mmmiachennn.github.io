---
title: Virtual Environment
tags:
  - python
categories:
  - Develop
date: 2025-04-21
draft: true
---

## 請去研究 uv

https://docs.astral.sh/uv/guides/scripts/#running-a-script-with-dependencies

## 應該要被歸類在 Clean code

https://medium.com/%E6%89%8B%E5%AF%AB%E7%AD%86%E8%A8%98/meaningful-names-893d3ae05ec

## 安裝與虛擬環境




###### tags: `Python`


## python 安裝

### 有線安裝

### 離線安裝
1. Download python
```
wget https://www.python.org/ftp/python/3.11.3/Python-3.11.3.tgz

tar -xvf Python-3.11.3.tgz
```

2. Build and install
```
cd Python-3.11.3
./configure --enable-optimizations
# 法 1
make -j $(nproc)
make altinstall
# 法 2
make
sudo make install

python3.11 --version
```

3. python path in /usr/local/bin 
4. 系統的 python /usr/bin


## 離線安裝 python 套件

```
# 如果是 tar.gz 檔案，解壓縮
cd xxx.tar.gz
python setup.py install

# 如果是 whl 檔案
pip install xxxx.whl

```

## python 虛擬環境套件推薦

https://sspai.com/post/75978


## 透過 proxy 安裝 python package
```
pip install --proxy http://xx.xx.xx.xx:port package
```

https://www.activestate.com/resources/quick-reads/pip-install-proxy/


=======================================
# pyenv + virtualenv

## Install
1. install linux dependence packages
```
sudo apt-get update; sudo apt-get install -y --no-install-recommends make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

2. github clone pyenv
```
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

pyenv offline install

https://gist.github.com/chewwt/c65a05959cf7dbb727ff3b76d8695be8

3. [pyenv environment path setting](https://github.com/pyenv/pyenv/tree/v2.4.7?tab=readme-ov-file#unixmacos)

4. github clone virtualenv to pyenv plugins
```
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
```

6. vitrualenv environment path setting
```
1 >> ~/.bashrc
```

7. source ~/.bashrc

## Activate

### install python

- 檢查可供安裝的 python 版本
```
pyenv install -l
```

- 有線安裝 
```
pyenv install 3.8.12

# result
Downloading Python-3.8.12.tar.xz...
-> https://www.python.org/ftp/python/3.8.12/Python-3.8.12.tar.xz
Installing Python-3.8.12...
```

- 離線安裝 [referance](https://juejin.cn/post/7167731442418941983)
```
# Download 
wget https://www.python.org/ftp/python/3.8.12/Python-3.8.12.tar.xz

# cp to .pyenv/cache folder
mkdir .pyenv/cache
cp Python-3.8.13.tar.xz .pyenv/cache

# install python
pyenv install 3.8.12
```

## virtualenv environment
```
# create
pyenv virtualenv 3.8.12 myenv

pyenv versions

# activate
pyenv activate myenv

# check activate
pyenv versions

# deactivate
pyenv deactivate myenv

# remove
pyenv uninstall myenv
```



## check version

- check pyenv version: pyenv --version
- check env: pyenv versions


## 虛擬環境目的

- by 專案/環境做套件相依性的管理
- python 版本更新
- 與系統 python 完全隔離


## 虛擬環境工具的比較

不同的工具有不同的使用場景。只有找到最適合自己的那個，才是好工具。
比如說提供新手開發使用 anaconda 最入門好上手。但如果是比較資深的工程師需要自己架環境且需要一直更新 python 版本使用 pyenv + virtualenv 就是一個方便快速的工具。
今天我想要 release 

由 chatGPT 歸納與整理

| 工具名稱         | 內建/需安裝                       | 依賴管理                | 環境隔離 | 支援 Python 版本        | 主要用途                           | 優點                                                        | 缺點                                             |
| ---------------- | --------------------------------- | ----------------------- | -------- | ----------------------- | ---------------------------------- | ----------------------------------------------------------- | ------------------------------------------------ |
| venv             | 內建 (Python 3.3+)                | pip                     | 是       | 只使用系統安裝的 Python | 一般開發、輕量級專案               | 內建於 Python，無需額外安裝，輕量、易用                     | 無法管理 Python 版本                             |
| virtualenv       | 需安裝 (pip install virtualenv)   | pip                     | 是       | 只使用系統安裝的 Python | 一般開發、與老舊 Python 版本兼容   | 比 venv 更快，支援舊版 Python                               | 額外安裝，與 venv 類似，功能略多但差異不大       |
| conda            | 需安裝 (Anaconda / Miniconda)     | conda (獨立於 pip)      | 是       | 可同時管理 Python 版本  | 資料科學、機器學習、大型專案       | 可管理 Python 版本 & 套件，適合科學計算，支援非 Python 套件 | 佔用空間較大，套件安裝可能較慢                   |
| pyenv-virtualenv | 需安裝 (pyenv + pyenv-virtualenv) | pip                     | 是       | 可獨立管理 Python 版本  | 需要同時管理 Python 版本與虛擬環境 | 與 pyenv 整合，可同時管理不同版本的 Python 和虛擬環境       | 設定較複雜，適合進階用戶                         |
| uv               | 需安裝 (pipx install uv)          | uv (獨立於 pip，速度快) | 是       | 只使用系統安裝的 Python | 依賴管理、加速 pip                 | 極快，比 pip、pip-tools 更快，支持 requirements.txt         | 仍在發展中，缺少部分高級功能                     |
| poetry           | 需安裝 (pipx install poetry)      | poetry (內建 lock 機制) | 是       | 只使用系統安裝的 Python | 現代 Python 專案管理               | 內建 lock file、自動管理 pyproject.toml，簡化 package 管理  | 速度比 uv 慢，與某些 Python 套件相容性可能有問題 |
|                  |                                   |                         |          |                         |                                    |                                                             |                                                  |

