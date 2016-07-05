### centOS 上部署云数据库服务

> sudo vi /etc/yum.repos.d/mongodb-org-3.0.repo

```

[mongodb-org-3.0]
name=MongoDB Repository
baseurl=http://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.0/x86_64/
gpgcheck=0
enabled=1

```
> 安装 sudo yum install -y mongodb-org

> 设置随系统启动运行 sudo chkconfig mongod on

> 启动服务 sudo service mongod start

> 配置项 vi /etc/mongod.conf

```
    
    设置可远程访问 bind_ip = 0.0.0.0 
    
```

> 打开27017端口防火墙
```

iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 27017 -j ACCEPT

```

> 重启服务 sudo service mongod restart

## 语法

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





