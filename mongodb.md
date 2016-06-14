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
    {$limit: 100}
])
```
