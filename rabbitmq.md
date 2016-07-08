##centos 安装

```
安装erlang
以root身份执行下面命令
yum install erlang
2
安装rabbitmq rpm包： 
wget http://www.rabbitmq.com/releases/rabbitmq-server/v3.5.0/rabbitmq-server-3.5.0-1.noarch.rpm
rpm -ivh rabbitmq-server-3.5.0-1.noarch.rpm 
3
启动rabbitmq，并验证启动情况 
rabbitmq-server --detached &ps aux |grep rabbitmq
4
以服务的方式启动
service rabbitmq-server start
5
检查端口5672是否打开
/sbin/iptables -I INPUT -p tcp --dport 5672 -j ACCEPT         
/etc/rc.d/init.d/iptables save      
/etc/init.d/iptables restart       
/etc/init.d/iptables status
centos 安装 rabbitmq
centos 安装 rabbitmq
6
启用维护插件：
rabbitmq-plugins enable rabbitmq_management 
centos 安装 rabbitmq
7
重启rabbitmq
service rabbitmq-server restart
centos 安装 rabbitmq
8
UI界面 http://ip:15672/  用户名密码 guest
无法登陆解决办法
vim /etc/rabbitmq/rabbitmq.config
写入信息，并保存
[{rabbit, [{loopback_users, []}]}].

```


### 启动server

> sbin/rabbitmq-server

### 后台启动server

> sbin/rabbitmq-server -detached

### 停止server

> rabbitmqctl stop 

### 查看状态

> rabbitmqctl status 

### 启动网页版控制台

> rabbitmq-plugins enable rabbitmq_management

> http://127.0.0.1:15672/

### 在node中使用 send

```
  var mq_conf = require('../../../config/rabbitmq.js');
  var rabbitmq_connect_str = 'amqp://' + mq_conf.user + ':' + mq_conf.password + '@' + mq_conf.host + '/'; 
  module.exports = function() {
    var url = rabbitmq_connect_str;
    amqp.connect(url, function(err, conn) {
      if (err) {
        return console.log(err)
      };
      conn.createChannel(function(err, chn) {
        if (err) {
          console.log(err);
        };
        var q = 'helloworld1';

        var data = {} ;

        data.name = 'ldl';

        chn.assertQueue(q, {durable: true});
        chn.sendToQueue(q, new Buffer(JSON.stringify(data)));
        chn.ack();
      });
    });
  };
```

### 在node中使用recieve
```
  var mq_conf = require('../../../config/rabbitmq.js');
  var rabbitmq_connect_str = 'amqp://' + mq_conf.user + ':' + mq_conf.password + '@' + mq_conf.host + '/'; 
  var bill_dao = require('@i5ting/hz-dao-billing').bill_dao;
  module.exports = function () {

    amqp.connect(rabbitmq_connect_str, function(err, conn) {
      console.log(err);
      conn.createChannel(function(err, ch) {
        console.log(err);
        var q = 'helloworld1';

        ch.consume(q, function(msg) {
          console.log(msg.content.toString());

          bill_dao.create(JSON.parse(msg.content.toString()));

          ch.ack(msg);
          
        }, {noAck: false});
      });
    });
  }
```
