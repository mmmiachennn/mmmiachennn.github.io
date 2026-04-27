---
title: Screen Command
tags:
  - redis
categories:
  - Common Command
date: 2026-04-21
draft: true
---


- 進 redis：
```
> redis-cli -h ms-node-01.venraas.private
```
- 尋找符合特定 pattern 的 key：
```
> scan 53411840 match *DAAZ* count 200
1) "15499264"
2) 1) "[\"pchome_mod_20250715\",\"goods_category_flatten\",\"DAAZ74-A900BZ396\"]"
```
- 查詢 redis 中有多少 key 值：
```
> DBSIZE
(integer) 53,493,153
```

- 查詢特定的 key
```
> lrange 'key' 0 -1
```

- 針對特定的 key 進行 value 的修改
```
> lpush 'key' 'value'
```