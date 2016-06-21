### 聚合的用法
```
db.getCollection('aa').aggregate([
    {$match : {
        "a" : "2016",
        "b" : "06",
        "c" : "14"
    }},
    {$group : {
        _id : "$a",count : {$sum : 1}
    }},
    {$limit: 10}
])
```

### 嵌套对象查询+模糊查询
```
db.getCollection('aaa').find({'a.b':/ems/})
```

### 排序

```
db.getCollection('order_new').find({ },{}).sort({_id: -1})
```





