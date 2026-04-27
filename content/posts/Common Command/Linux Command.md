---
title: Linux Command
tags:
  - linux
categories:
  - Common Command
date: 2026-04-21
draft: true
---



## command line

```
# 跳至最前面
ctrl + A

# 跳至最後面
ctrl + E
```

## find 語法
```
1. find .: 這個命令告訴 find 從當前目錄 (.) 開始向下搜尋。
2. -type d: 這個選項告訴 find 只尋找目錄（directory）。
3. -name "2025-03-*": 這個選項告訴 find 尋找名稱以 "2025-03-" 開頭的檔案或目錄。由於我們已經使用了 -type d，所以這裡只會匹配到符合這個名稱模式的資料夾。
4. -mindepth 1: 最少查詢深度？
5. -empty： 找空的資料夾
6. -delete： 執行刪除
7. -mtime： 檔案或資料夾的更新時間
```

### 單純搜尋
```
find . -type d -name "2025-03-*"
```

### 刪除指定路徑下空的資料夾，並保留資料夾本身
```
find "${dag_dir}" -mindepth 1 -type d -empty -delete
```

### 刪除指定路徑下超過 EXPIRED_DAYS
```
find "${dag_dir}" -type f -mtime +${EXPIRED_DAYS} -delete
```

## 檔案與目錄

### 切換目錄
```
# 切換至「之前的目錄」
cd -
# 切換至家目錄
# cd ~
```

### 相對路徑
```
cd ../postfix
```

### 複製資料夾/檔案
```
# Pictures 裡面的所有內用複製到 Picture2
cp -R Pictures/* Picture2

# Pictures 整個資料夾複製到 Picture2
cp -R Picture/ Picture2

# Pictures 中所有的 .jpg 檔案複製至 Picture2
cp Picture/*.jpg Picture2

# 複製檔案
cp doc.txt Picture2/doc2.txt
cp doc.txt Picture2/
```

### 查看資料夾內容
```
ls
ls -a #包含隱藏檔案
ls -l #每行列出一個文件
ls -al #所有文件的長格式列表（權限，所有權，大小和修改日期）
```

### 壓縮
```
tar -zcvf filename.tar.gz filename
# 省略 .git .idea 與版本控制有關的檔案
tar --exclude-vcs -zcvf filename.tar.gz
# 指定目錄壓縮
tar -zcvf filename.tar.gz -C folder_name/file_or_folder_name
```

### 解壓縮
```
tar -zxvf FileName.tar.gz
# 解壓縮至指定目錄
tar -zxvf filename.tar.gz -C folder_name
```

## 檔案進階應用

### 計算檔案個數
```
>> ls | grep "202203" | wc -l
>> 4
```

### 查詢 csv row
```
wc -l filename
```

### 排除一些定義檔案（參考資料）
```
tar --exclude-vcs
```

### print csv 前 10 row
```
awk 'FNR==NR{next} FNR==1{n=NR-1} FNR>x || FNR==n{x+=n/9;print}' db.csv db.csv
```

### 大量刪除 v1_2022 字串開頭的檔案
```
rm -f $(ls | grep "^v1_2022")
```

## 權限相關

權限代碼意義
```
-（代表類型）×××（所有者）×××（組用戶）×××（其他用戶）

+ 表示增加權限、- 表示取消權限、= 表示唯一設定權限。
r 表示可讀取，w 表示可寫入，x 表示可執行，X 表示只有當該檔案是個子目錄或者該檔案已經被設定過為可執行。
-c : 若該檔案權限確實已經更改，才顯示其更改動作
-f : 若該檔案權限無法被更改也不要顯示錯誤訊息
-v : 顯示權限變更的詳細資料
-R : 對目前目錄下的所有檔案與子目錄進行相同的權限變更(即以遞回的方式逐個變更)

ls -al 查看所在目錄之檔案權限
chmod 更改權限
```

## 連線相關
### 遠端連線
```
ssh username@host
password
```

### 檔案傳輸
```
scp local_path/fileName username@host:path
```


## 檢視機器
### 環境設定

- /etc/hosts
- ~/.bashrc
- ~/.profile

### 查看 port 使用情況
```
sudo netstat -tnlp
sudo netstat -pna | grep 15672
sudo kill 上面找到的id
```

### memory usage 
```
# PS 指令  + 排序
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head
```

### top 指令
https://kknews.cc/zh-tw/code/m6ylmp6.html

## apt-get
### 查詢安裝的套件名稱
```
dpkg --list
```

### 移除安裝的套件
```
sudo apt-get --purge remove 套件名稱
```

### 安裝 deb 檔案
```
sudo apt install slack.......deb
```

### 更新 chorme 
```
sudo apt-get update 
sudo apt-get --only-upgrade install google-chrome-stable.
```


Bash指令
https://linux.die.net/man/ /bas