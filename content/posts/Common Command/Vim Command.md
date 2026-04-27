---
title: Vim Command
tags:
  - linux
categories:
  - Common Command
date: 2026-04-21
draft: true
---

## 指令整理 

https://blog.vgod.tw/2009/12/08/vim-cheat-sheet-for-programmers/

### Copy (將 y 換成 d 即爲剪下)
```
yy	# 複製當前游標所在的行，包括換行符。 
2yy	# 從游標所在的行開始複製兩行 
y$	# 從游標所在的位置開始複製所有的內容，直到行尾。 
y^	# 從游標所在的位置開始複製所有的內容到行的開始 
yw	# 複製從游標所在的位置開始的所有內容到另一個詞的開始 
yiw	# 複製當前單詞 
y%	# 複製匹配字元之間的文字，如括號，例如，用於複製 ( ) 之間的所有內容。
```

### Paste
```
p # 將文字貼上在游標之後
P # 將文字貼上在游
```
### 
跳至行尾
```
:$
```

### 跳至行頭
```
:0
```

### To Bottom
```
:G
```

### To Top
```
:1G
```

### Find
```
/find_string

type enter

# find next
type n

# find previous
type N
```

### Quit the file
```
:q
```

### Quit and save change
```
:wq
```

### Force Quit the file
```
:q!
```

### Replace 
string -> replace
```
:%s/string/replace
```

### 清空所有內容
```
ggdG
```
