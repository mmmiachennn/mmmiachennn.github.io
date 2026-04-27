---
title: ClickHouse
tags:
  - OLAP
categories:
  - Develop
date: 2025-04-21
draft: true
---

### Mongo 系統時間設定

https://dotblogs.com.tw/takalearingnotes/2018/07/17/104049

### Install in Ubuntu
https://wayne265265.pixnet.net/blog/post/117991769

### 超強 DS MONGO 常用指令

https://towardsdatascience.com/manipulating-data-with-mongodb-bd561f09d76a


### mongo 基本觀念

https://ithelp.ithome.com.tw/articles/10187503


### Index 的建立

https://ithelp.ithome.com.tw/articles/10187503


### Connect

```
mongo -u uesr -p password host:port/dbname
```


### Dump 

#### database

```
mongodump -h host -u user -p password -d dbname -o local_path
# 抓取部分資料
mongodump -h host -u user -p password -d dbname -c collection -q '{"field": ...}' -o local_path
mongodump -h host -u user -p password -d dbname -c collection -q '{"field": {"$gt": {"$date": "2022-02-01T00:00:00.000Z"}}}' -o local_path
# 遇到 
mongodump --forceTableScan -h host -u user -p password -d dbname -o local_path
# 邊 dump 邊壓縮
mongodump --forceTableScan -h host -u user -p password -d dbname --gzip -o local_path
```

#### collection

```
mongodump -h host -u user -p password -d dbname -c collection -o local_path
mongodump --forceTableScan -h host -u user -p password -d dbname -c collection -o local_path
```

#### view dump to collection

```
mongodump -h host -u user -p password -d dbname -c viewname --viewsAsCollections -o local_path
```


### Restore

#### database

```
mongorestore -h host -u user -p password -d dbname local_path
# 回復前先刪除資料
mongorestore -u user -p password -d dbname --drop local_path
# restore 壓縮檔
mongorestore -h host -u user -p password -d dbname local_path --gzip
```


#### collection

```
mongorestore -h host -u user -p passward -d dbname -c local_path
```

如果出現  db server: server returned error on SASL authentication step: Authentication failed. ，可以在 -d database 後面添加 
```
--authenticationDatabase admin
```


### Import


#### collection

```
mongoimport -h host -u admin -p admin -d dbname -c collection --type csv --file local_path/file --headerline
```


### Create User

```
db.createUser({  
 user:"admin",
 pwd:"admin",
 roles:[  
  {  
     role:"readWrite",
     db:"prd_unimicron"
  }
 ],
 mechanisms:[  
  "SCRAM-SHA-1"
 ]
})
```


### Drop collection

```
db.collection.drop()
```


### Filter Interval

```
$gt # greate then
$gte # equal or greate then
$lt # small then
$lte #equal or small then
```


### About Index

```
db.collection.createIndex({field: 1, filed:-1})
db.collection.getIndexes()
db.collection.dropIndex("index_name")
```


### Sort

```
db.collection.find().sort({age:-1}).limit(1) // for MAX
db.collection.find().sort({age:+1}).limit(1) // for MIN
```


### Create New DataBase

1. 使用最高權限進去DB 中 mongo -u username -p password host:27017/dbname --authenticationDatabase admin
2. use newDatabase  # 創建一個新的database
3. create new collection and insert documents
4. createUser add new use and passwordd


### Aggregate Lookup (Mapping Table select field)

https://www.mongodb.com/community/forums/t/how-to-add-single-field-using-lookup/104792


### Aggregate Lookup (Mapping Table result output collection)

https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/
```
db.getSiblingDB("test").books.aggregate([{
    $group: {
        _id: "$author", 
        books: {
            $push: "$title"
        }
    }},
    {
    $out: "authors"}
])
```


```
# data
db.orders.insertMany( [
   { "_id" : 1, "item" : "almonds", "price" : 12, "quantity" : 2 },
   { "_id" : 2, "item" : "pecans", "price" : 20, "quantity" : 1 }
] )

db.items.insertMany( [
  { "_id" : 1, "item" : "almonds", description: "almond clusters", "instock" : 120 },
  { "_id" : 2, "item" : "bread", description: "raisin and nut bread", "instock" : 80 },
  { "_id" : 3, "item" : "pecans", description: "candied pecans", "instock" : 60 }
] )

# command
db.orders.aggregate( [
   {
      $lookup: {
         from: "items",
         localField: "item",    // field in the orders collection
         foreignField: "item",  // field in the items collection
         as: "fromItems"
      }
   },
   {
      $replaceRoot: { newRoot: { $mergeObjects: [ { $arrayElemAt: [ "$fromItems", 0 ] }, "$$ROOT" ] } }
   },
   { $project: { fromItems: 0 } }
] )

# result
{
  _id: 1,
  item: 'almonds',
  description: 'almond clusters',
  instock: 120,
  price: 12,
  quantity: 2
},
{
  _id: 2,
  item: 'pecans',
  description: 'candied pecans',
  instock: 60,
  price: 20,
  quantity: 1
}
```



### 參考網站：https://docs.mongodb.com/manual/reference/method/
