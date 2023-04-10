# 02 连接MongoDB

使用默认端口来连接 MongoDB 的服务。

```shell
mongodb://localhost/
```

## 连接格式

```shell
mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
```

- **mongodb://** 这是固定的格式，必须要指定。
- **username:password@** 可选项，如果设置，在连接数据库服务器之后，驱动都会尝试登录这个数据库
- **host1** 必须的指定至少一个host, host1 是这个URI唯一要填写的。它指定了要连接服务器的地址。如果要连接复制集，请指定多个主机地址。
- **portX** 可选的指定端口，如果不填，默认为27017
- **/database** 如果指定username:password@，连接并验证登录指定数据库。若不指定，默认打开 test 数据库。
- **?options** 是连接选项。如果不使用/database，则前面需要加上/。所有连接选项都是键值对name=value，键值对之间通过&或;（分号）隔开

### 参数选项说明

标准的连接格式包含了多个选项(options)，如下所示：  

| 选项                  | 描述                                                                                                                                                                                                       |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| replicaSet=name     | 验证replica set的名称。 Impliesconnect=replicaSet.                                                                                                                                                             |
| slaveOk=true\|false | - true:在connect=direct模式下，驱动会连接第一台机器，即使这台服务器不是主。在connect=replicaSet模式下，驱动会发送所有的写请求到主并且把读取操作分布在其他从服务器。<br>- false: 在 connect=direct模式下，驱动会自动找寻主服务器. 在connect=replicaSet 模式下，驱动仅仅连接主服务器，并且所有的读写命令都连接到主服务器。 |
| safe=true\|false    | - true: 在执行更新操作之后，驱动都会发送getLastError命令来确保更新成功。(还要参考 wtimeoutMS).<br><br>false: 在每次更新之后，驱动不会发送getLastError来确保更新成功。                                                                                        |
| w=n                 | 驱动添加 { w : n } 到getLastError命令. 应用于safe=true。                                                                                                                                                            |
| wtimeoutMS=ms       | 驱动添加 { wtimeout : ms } 到 getlasterror 命令. 应用于 safe=true.                                                                                                                                                 |
| fsync=true\|false   | - true: 驱动添加 { fsync : true } 到 getlasterror 命令.应用于 safe=true.<br>- false: 驱动不会添加到getLastError命令中。                                                                                                       |
| journal=true\|false | 如果设置wie true, 同步到 journal (在提交到数据库前写入到实体中). 应用于 safe=true                                                                                                                                                |
| connectTimeoutMS=ms | 可以打开连接的时间。                                                                                                                                                                                               |
| socketTimeoutMS=ms  | 发送和接受sockets的时间。                                                                                                                                                                                         |

### 连接实例

使用用户名fred，密码foobar登录localhost的admin数据库。

```shell
mongodb://fred:foobar@localhost
```

使用用户名fred，密码foobar登录localhost的baz数据库。

```shell
mongodb://fred:foobar@localhost/baz
```

连接 replica pair, 服务器1为example1.com服务器2为example2。

```shell
mongodb://example1.com:27017,example2.com:27017
```

连接 replica set 三台服务器 (端口 27017, 27018, 和27019):

```shell
mongodb://localhost,localhost:27018,localhost:27019
```

连接 replica set 三台服务器, 写入操作应用在主服务器 并且分布查询到从服务器。

```shell
mongodb://host1,host2,host3/?slaveOk=true
```

直接连接第一个服务器，无论是replica set一部分或者主服务器或者从服务器。

```shell
mongodb://host1,host2,host3/?connect=direct;slaveOk=true
```

当你的连接服务器有优先级，还需要列出所有服务器，你可以使用上述连接方式。

安全模式连接到localhost:

```shell
mongodb://localhost/?safe=true
```

以安全模式连接到replica set，并且等待至少两个复制服务器成功写入，超时时间设置为2秒。

```shell
mongodb://host1,host2,host3/?safe=true;w=2;wtimeoutMS=2000
```
