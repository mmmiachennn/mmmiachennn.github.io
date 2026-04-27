---
title: 什麼是 Jupyter Lab?
tags:
  - python
categories:
  - 開發相關
date: 2021-04-12
draft: true
---

## Jupyter Notebook vs Jupyter Lab ?
身為一個資料科學家常常在使用 Jupyter Notebook，在發現 Jupyter Lab 之後，二話不說直接轉跳過去，
Jupyter Lab 和 Jupyter Notebook 應該是好兄弟吧（我是這樣覺得），
Jupyter Lab 有很多附加功能，可拖拉 kernel 的位置、可以同時看很多個檔案、可直接查看 csv 檔案...等，
非常推薦使用 Jupyter Notebook 的人轉換成 Jupyter Lab。

---
## Jupyter Lab 更換 code 文字顏色顏色
對一個眼睛不好但必須常常盯著電腦的人來說，白色主題實在是太殘酷了。
Jupyter Lab 就有提供兩種最常見的主題顏色(黑/白)，
但是它的黑色主題配色本人實在不喜歡，所以我搜尋了如何轉換字的顏色的方法。


- 首先是尋找該路徑下的 index.css 的檔案。
```
~/anaconda3/envs/best/share/jupyter/lab/themes/@jupyterlab/theme-dark-extension/index.css
```
- 主要 code 顏色從 line 322 開始，色碼使用為 hex color。
可直接搜尋 hex color，Google 會有調色盤輸入色碼可以直接看到相對應的顏色，
或是直接點選喜歡的顏色會跳出色碼。
```
 /* Code mirror specific styles */

  --jp-mirror-editor-keyword-color: var(--md-green-500);
  --jp-mirror-editor-atom-color: var(--md-blue-300);
  --jp-mirror-editor-number-color: var(--md-green-400);
  --jp-mirror-editor-def-color: var(--md-blue-600);
  --jp-mirror-editor-variable-color: var(--md-grey-300);
  --jp-mirror-editor-variable-2-color: var(--md-blue-400);
  --jp-mirror-editor-variable-3-color: var(--md-green-600);
  --jp-mirror-editor-punctuation-color: var(--md-blue-400);
  --jp-mirror-editor-property-color: var(--md-blue-400);
  --jp-mirror-editor-operator-color: #aa22ff; # 運算符號的顏色
  --jp-mirror-editor-comment-color: #408080;
  --jp-mirror-editor-string-color: #ba2121; # String 的顏色
  --jp-mirror-editor-string-2-color: var(--md-purple-300);
  --jp-mirror-editor-meta-color: #aa22ff;
  --jp-mirror-editor-qualifier-color: #555;
  --jp-mirror-editor-builtin-color: var(--md-green-600);
  --jp-mirror-editor-bracket-color: #997;
  --jp-mirror-editor-tag-color: var(--md-green-700);
  --jp-mirror-editor-attribute-color: var(--md-blue-700);
  --jp-mirror-editor-header-color: var(--md-blue-500);
  --jp-mirror-editor-quote-color: var(--md-green-300);
  --jp-mirror-editor-link-color: var(--md-blue-700);
  --jp-mirror-editor-error-color: #f00;
  --jp-mirror-editor-hr-color: #999;
```

參考資料：https://github.com/jupyterlab/jupyterlab/issues/8158


### jupyterlab 遠端連線
port 應該可以自己指定， --allow-root 忘記是什麼了
```
jupyter lab --no-browser --ip 0.0.0.0 --port 9453 --allow-root

# 內網直接連線
192.168.66.31:9453
```

### Virtualenv

```
pip install virtualenv
virtualenv --python=/opt/python-3.6/bin/python venv
source ./venv/bin/activate
```

create virtual environment

```
cd dictionary
virtualenv envName
```

activate/deactivate

```
source envName/bin/activate
deactivate
```


