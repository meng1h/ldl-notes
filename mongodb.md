### 聚合的用法
```
db.getCollection('stockouts').aggregate([
    {$match : {
        "created_year" : "2016",
        "created_month" : "06",
        "created_day" : "14"
    }},
    {$group : {
        _id : "$outer_order_id",count : {$sum : 1}
    }},
    {$limit: 100}
])
```
