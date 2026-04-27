---
title: Shell Script Command
tags:
  - linux
categories:
  - Common Command
date: 2026-04-21
draft: true
---

shell script  
https://linux.vbird.org/linux_basic/centos7/0340bashshell-scripts.php#ifthen

### 截取變數部分字串
```
start_date=20231202
echo "${start_date:4:2}"  # 12
echo "${start_date:0:4}" # 2023
```

### 執行 shell script 的時候遇到以下問題
```
bash: $'\r': command not found
# 解決辦法
sed -i 's/\r$//' build.sh
# or
dos2unix file.sh
```

### 取代的用法
```
#!/bin/bash 
CASE_NAME="$1" 
REPLACE="-" 
JOB_NAME="ec-token-make-${CASE_NAME//_/$REPLACE}" 
echo $JOB_NAME
```


 ### 抓取參數
```
# 当前脚本的文件名
$0

# 传递给脚本或函数的参数。n 是一个数字，表示第几个参数。例如，第一个参数是$1，第二个参数是$2。
$n

# 传递给脚本或函数的参数个数。
$#

# 传递给脚本或函数的所有参数。
$*

# 传递给脚本或函数的所有参数。被双引号(" ")包含时，与 $* 稍有不同，下面将会讲到。
$@

# 上个命令的退出状态，或函数的返回值。
$?

# 当前Shell进程ID。对于 Shell 脚本，就是这些脚本所在的进程ID。
$$
```
example
```
#!/bin/bash
echo "File Name: $0"
echo "First Parameter : $1"
echo "First Parameter : $2"
echo "Quoted Values: $@"
echo "Quoted Values: $*"
echo "Total Number of Parameters : $#"
```


### 直接 assign 一個環境變數,只是可能執行階段結束就不見了
var=value bash 123.sh 
var=value sh -c 'echo "$var"'
bash 123.sh value
var="$1"
echo "$var"

你想要執行完就不見 安全性需求不想讓別人看到變數
你想要永久寫在機器變數上 偶爾可以調整還要可以方便調整
你想要每臺機器的同一個變數值不一樣