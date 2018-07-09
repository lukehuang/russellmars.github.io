title: mongodb replica set 搭建
date: 2015-08-07 09:28:09
tags: mongodb
categories: mongodb
---
以下环境在CentOS 64-bit虚拟机中验证通过，测试时有些命令是需要root权限的，并且我把虚拟机中的防火墙关了，然后我可以再我的宿主机中用java通过mongodb的驱动访问访问虚拟机中的ip的端口。
<!--more-->
## 防火墙的相关方法：
service iptables status可以查看到iptables服务的当前状态
重启后生效
开启： chkconfig iptables on
关闭： chkconfig iptables off
即时生效，重启后失效
开启： service iptables start
关闭： service iptables stop 
## mongodb replSet 搭建
### 创建数据库存放目录
建立复制集群节点的数据存放目录
mkdir -p /opt/mongodata/r1
mkdir -p /opt/mongodata/r2
mkdir -p /opt/mongodata/r3
### 启动mongod
在mongodb目录下分别打开3个终端，执行如下命令：
bin/mongod --dbpath /opt/mongodata/r1 --port 27018 --rest --replSet myset
bin/mongod --dbpath /opt/mongodata/r2 --port 27019 --rest --replSet myset
bin/mongod --dbpath /opt/mongodata/r3 --port 27020 --rest --replSet myset
### 初始化集群的信息
在打开一个终端，执行如下命令
bin/mongo 127.0.0.1:27018 init.js
init.js的内容如下：
rs.initiate({
    _id : "myset",
    members : [
        {_id : 0, host : "192.168.126.128:27018",priority:3},
        {_id : 1, host : "192.168.126.128:27019",priority:2},
        {_id : 2, host : "192.168.126.128:27020",priority:1}
    ]
})

此时已经建立了replica set 的集群部署
## 验证replica set
### 查看集群状态
打开终端，连接集群中的其中一个节点
bin/mongo 127.0.0.1:27018

myset:PRIMARY> rs.status()
{
	"set" : "myset",
	"date" : ISODate("2015-08-06T14:32:39.525Z"),
	"myState" : 1,
	"members" : [
		{
			"_id" : 0,
			"name" : "192.168.126.128:27018",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 37,
			"optime" : Timestamp(1438861784, 2),
			"optimeDate" : ISODate("2015-08-06T11:49:44Z"),
			"lastHeartbeat" : ISODate("2015-08-06T14:32:38.524Z"),
			"lastHeartbeatRecv" : ISODate("2015-08-06T14:32:38.194Z"),
			"pingMs" : 0,
			"configVersion" : 1
		},
		{
			"_id" : 1,
			"name" : "192.168.126.128:27019",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 11034,
			"optime" : Timestamp(1438861784, 2),
			"optimeDate" : ISODate("2015-08-06T11:49:44Z"),
			"lastHeartbeat" : ISODate("2015-08-06T14:32:37.742Z"),
			"lastHeartbeatRecv" : ISODate("2015-08-06T14:32:37.749Z"),
			"pingMs" : 0,
			"configVersion" : 1
		},
		{
			"_id" : 2,
			"name" : "192.168.126.128:27020",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY",
			"uptime" : 12788,
			"optime" : Timestamp(1438861784, 2),
			"optimeDate" : ISODate("2015-08-06T11:49:44Z"),
			"electionTime" : Timestamp(1438871418, 1),
			"electionDate" : ISODate("2015-08-06T14:30:18Z"),
			"configVersion" : 1,
			"self" : true
		}
	],
	"ok" : 1
}

state：1表示该host是当前可以进行读写，2：不能读写
health：1表示该host目前是正常的，0:异常

查看是否是主库的命令
myset:PRIMARY> db.isMaster()
{
	"setName" : "myset",
	"setVersion" : 1,
	"ismaster" : true,
	"secondary" : false,
	"hosts" : [
		"192.168.126.128:27018",
		"192.168.126.128:27019",
		"192.168.126.128:27020"
	],
	"primary" : "192.168.126.128:27020",
	"me" : "192.168.126.128:27020",
	"electionId" : ObjectId("55c36f7af1cf2b97bc876d6a"),
	"maxBsonObjectSize" : 16777216,
	"maxMessageSizeBytes" : 48000000,
	"maxWriteBatchSize" : 1000,
	"localTime" : ISODate("2015-08-06T14:54:18.977Z"),
	"maxWireVersion" : 3,
	"minWireVersion" : 0,
	"ok" : 1
}

### 读写验证
默认情况下，主库可以读写，从库无法读写
主库（primary）：
写：
myset:PRIMARY> db.person.insert({name:"katy",age:30})
WriteResult({ "nInserted" : 1 })
读：
myset:PRIMARY> db.person.find()
{ "_id" : ObjectId("55c375f0b31a759cb89eeb05"), "name" : "katy", "age" : 30 }
从库（secondary）：
写：
myset:SECONDARY> db.person.insert({name:"mars",age:33})
WriteResult({ "writeError" : { "code" : undefined, "errmsg" : "not master" } })
读：
myset:SECONDARY> db.person.find()
Error: error: { "$err" : "not master and slaveOk=false", "code" : 13435 }

设置从库可读（在连接从库的终端中执行）
myset:SECONDARY> db.getMongo().setSlaveOk()
验证：
myset:SECONDARY> db.person.find()
{ "_id" : ObjectId("55c349d7cb6cfe2ee4fcf613"), "name" : "mars", "age" : 33 }
{ "_id" : ObjectId("55c375f0b31a759cb89eeb05"), "name" : "katy", "age" : 30 }

### 故障转移测试
之前集群时的设置如下：
members : [
        {_id : 0, host : "192.168.126.128:27018",priority :3},
        {_id : 1, host : "192.168.126.128:27019",priority :2},
        {_id : 2, host : "192.168.126.128:27020",priority :1}
    ]
服务开启后，
27018的节点成为primary，其他的为secondary
kill27018后，27019成为primary
启动27018后，27018依然是primary,27019成为secondary
### 查看集群的配置
rs.config()
如果要修改集群中节点的优先级，则必须在primary进程的客户端中设置如下：
myset:PRIMARY> config = rs.conf()
myset:PRIMARY> config.members[0].priority = 3
myset:PRIMARY> rs.reconfig(config)

## 参考文献
【Mongodb】如何创建mongodb的replica set（http://blog.itpub.net/22664653/viewspace-710004/）