### 系统环境

```shell
 [root@node1 ~]# cat /etc/redhat-release
CentOS Linux release 7.4.1708 (Core)
 [root@node1 ~]# uname -r
3.10.0-693.el7.x86_64
 [root@node1 ~]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(1)
 [root@node1 ~]# getenforce
Disabled
 [root@node1 ~]# hostname -i
192.168.11.50
```

### 目录规划

Redis下载目录

```shell
/server/tools
```

Redis安装目录

```shell
/opt/redis_cluster/redis_{PORT}/{conf,logs,pid}
```

Redis数据目录

```shell
/data/redis_cluster/redis_{PORT}/redis_{PORT}.rdb
```

Redis运维脚本

```shell
/server/scripts/redis_shell.sh
```

### 安装Redis

 创建下载目录，安装目录和数据目录

```shell
mkdir -p /server/tools
mkdir -p /data/redis_cluster/redis_6379
mkdir -p /opt/redis_cluster/redis_6379/{conf,logs,pid}
```

下载软件和创建软链接

```shell
cd /server/tools
wget http://download.redis.io/releases/redis-3.2.9.tar.gz
tar zxf redis-3.2.9.tar.gz -C /opt/redis_cluster
ln -s /opt/redis_cluster/redis-3.2.9/ /opt/redis_cluster/redis
```

编译安装

```shell
cd /opt/redis_cluster/redis
make && make install
```

### 设置Redis systemctl启动的两种方式

方式一：自定义写

```shell
vim /lib/systemd/system/redis.service        
[Unit]
Description=redis-server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/redis-server /opt/redis_cluster/redis_6379/conf/6379.conf #自定义
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

方式二：使用Redis自带的定义

```shell
cd /opt/redis_cluster/redis/utils
./install_server.sh  #执行脚本
Welcome to the redis service installer
This script will help you easily set up a running redis server

Please select the redis port for this instance: [6379] 6379   #输入定义的redis端口
Please select the redis config file name [/etc/redis/6379.conf]   /opt/redis_cluster/redis_6379/conf/6379.conf   #配置文件
Please select the redis log file name [/var/log/redis_6379.log] /opt/redis_cluster/redis_6379/logs/redis_6379.log   #日志目录
Please select the data directory for this instance [/var/lib/redis/6379] /data/redis_cluster/redis_6379  #数据目录
Please select the redis executable path [/usr/local/bin/redis-server]  #默认
Selected config:
Port           : 6379
Config file    : /opt/redis_cluster/redis_6379/conf/6379.conf
Log file       : /opt/redis_cluster/redis_6379/logs/redis_6379.log
Data dir       : /data/redis_cluster/redis_6379
Executable     : /usr/local/bin/redis-server
Cli Executable : /usr/local/bin/redis-cli
Is this ok? Then press ENTER to go on or Ctrl-C to abort.
Copied /tmp/6379.conf => /etc/init.d/redis_6379
Installing service...
Successfully added to chkconfig!
Successfully added to runlevels 345!
Starting Redis server...
Installation successful!
```

会将配置生成/etc/init.d/redis_6379 启动脚本，使用stop|start|restart 即可

将redis.conf设置成后台启动，否则使用systemctl启动时会一直卡着`

### Redis配置说明

```shell
vim /opt/redis_cluster/redis_6379/conf/6379.conf
# 以守护进程模式启动
daemonize yes

# 绑定的主机地址
bind 192.168.11.50

# 监听的端口
port 6379

# pid文件和log文件的保存地址
pidfile /opt/redis_cluster/redis_6379/pid/redis_6379.pid
logfile /opt/redis_cluster/redis_6379/logs/redis_6379.log

# 设置数据库的数量，默认数据库为0
databases 16

# 指定本地持久化文件的文件名，默认是dump.rdb
dbfilename redis_6379.rdb

# 本地数据库的目录
dir /data/redis_cluster/redis_6379
```

### Redis基本操作命令-全局命令

Redis有5种

### Redis主从复制

在分布式系统中为了解决单点问题，通常会把数据复制多个副本到其他机器，满足故障恢复和负载均衡等需求，Redis也是如此，提供了复制功能

复制功能是高可用Redis的基础，后面的哨兵和集群都是在复制的基础上实现高可用的

**主从复制流程**

* 从服务器向主服务器发送 SYNC 命令。
* 接到 SYNC 命令的主服务器会调用BGSAVE 命令，创建一个 RDB 文件，并使用缓冲区记录接下来执行的所有写命令。
* 当主服务器执行完 BGSAVE 命令时，它会向从服务器发送 RDB 文件，而从服务器则会接收并载入这个文件。
* 主服务器将缓冲区储存的所有写命令发送给从服务器执行。

![](http://images.dregs.top/images/20190820125325.png)

**注意：**

> 1、在开启主从复制的时候，使用的是RDB方式的，同步主从数据的
> 2、同步开始之后，通过主库命令传播的方式，主动的复制方式实现
> 3、2.8以后实现PSYNC的机制，实现断线重连

**建立主从复制的三种方式：**

每个从节点只能有一个主节点，主节点可以有多个从节点。

* 在配置文件中加入slaveof {masterHost} {masterPort}随Redis启动生效
* 在Redis-server启动命令后加入一slaveof  {masterHost} {masterPort} 生效
* 直接使用命令：slaveof {masterHost} {masterPort} 生效

查看复制状态信息命令

```shell
info replication
```

**断开复制**

slaveof命令不但可以建立复制，还可以在从节点上执行`slave of no noe`来断开与主节点复制关系

断开复制主要流程：

1. 断开与主节点复制关系
2. 从节点晋升为主节点，从节点断开复制后不会抛弃原有数据，只是无法再获取主节点上的数据变化

**切换主节点**

通过slaveof 命令还可以实现切主操作，所谓切主是指把当前从节点对主节点的复制切换到另一个主节点上

执行slaveof {newMasterIp} {newMasterPort}命令即可

切主操作流程如下：

1. 断开旧主节点的复制关系
2. 与新主节点建立复制关系
3. 删除从节点当前所有数据
4. 对新主节点进行复制操作

> 提示：线上操作一定要小心，因为切主后会清空之前所有的数据

**Redis开启主从复制**

从节点安装跟上面步骤一样，也可以直接将现有的redis拷贝过去也可以

创建数据目录

```shell
mkdir -p /server/tools
mkdir -p /data/redis_cluster/redis_6379
mkdir -p /opt/redis_cluster/redis_6379/{conf,logs,pid}
```

在11.50上拷贝文件到51

```shell
scp -r /opt/redis_cluster/ 192.168.11.51:/opt
scp /usr/local/bin/redis* 192.168.11.51:/usr/local/bin
```

修改配置文件

```shell
vim /opt/redis_cluster/redis_6379/conf/6379.conf
# 以守护进程模式启动
daemonize yes

# 绑定的主机地址
bind 192.168.11.51

# 监听的端口
port 6379

# pid文件和log文件的保存地址
pidfile /opt/redis_cluster/redis_6379/pid/redis_6379.pid
logfile /opt/redis_cluster/redis_6379/logs/redis_6379.log

# 设置数据库的数量，默认数据库为0
databases 16

# 指定本地持久化文件的文件名，默认是dump.rdb
dbfilename redis_6379.rdb

#主从复制 添加这行即可 主不用动
slaveof 192.168.11.50 6379

#本地数据库的目录
dir /data/redis_cluster/redis_6379
```

测试：在主set一个值，在从get能查看到就ok了

### Redis基本操作全局命令

 制作1000条数据

 ```shell
for i in {1..1000};do redis-cli -h 192.168.11.50 -p 6379 set key_${i} v_${i};done
 ```

查看所有的数据key,不是值，不建议使用,  

```shell
192.168.11.50:6379> keys *
```

推荐使用

```shell
192.168.11.50:6379> dbsize #直接返回多少条数据
```

检查键是否存在

```shell
192.168.11.50:6379> Exists key #如果存在返回1，不存在返回0
```

删除键

```shell
192.168.11.50:6379> Del key {key...}
```

键过期

```shell
192.168.11.50:6379> EXPIRE key_997 30  #30是过期时间
(integer) 1
192.168.11.50:6379> ttl key_997  #查看过期时间,查看一个不存在的键过期时间返回值为-2，-1这个键存在没设置过期时间
(integer) 25
```

### Redis数据类型之字符串操作

设置和获取字符串

```shell
192.168.11.50:6379> set key1 value1
OK
192.168.11.50:6379> get key1
"value1"
192.168.11.50:6379> keys * 
```

INCR命令将字符串解析成整型，将其加1，保存为新的字符串

```shell
192.168.11.50:6379> set key2 100
OK
192.168.11.50:6379> get key2
"100"
192.168.11.50:6379> incr key2
(integer) 101
192.168.11.50:6379> get key2
"101"
192.168.11.50:6379> incrby key2 10 #直接增加10
(integer) 111
192.168.11.50:6379> get key2
"111"
```

MSET和MGET可以一次性存储或获取多个key对应的值

```shell
192.168.11.50:6379> mset key3 v3 key4 v4 key5 v5
OK
192.168.11.50:6379> mget key3 key4 key5
1) "v3"
2) "v4"
3) "v5"
```

EXISTS命令返回 1或者0标记给定的key的值是否存在

使用DEL命令可以删除key对应的值

DEL命令返回1或0时被删除(值存在)或者没被删除(key对应的值不存在)

```shell
192.168.11.50:6379> EXISTS key5
(integer) 1
192.168.11.50:6379> del key5
(integer) 1
192.168.11.50:6379> EXISTS key5
(integer) 0
192.168.11.50:6379> del key5
(integer) 0
```

type命令可以返回key对应的存储类型

```shell
192.168.11.50:6379> set key5 v5
OK
192.168.11.50:6379> type key5
string
```

设置超时时间，当这个时间到达后被删除

```shell
192.168.11.50:6379> get key5
"v5"
192.168.11.50:6379> ttl key5
(integer) -1
192.168.11.50:6379> expire key5 10
(integer) 1
192.168.11.50:6379> ttl key5
(integer) 5
192.168.11.50:6379> ttl key5
(integer) 4
192.168.11.50:6379> ttl key5
(integer) 3
192.168.11.50:6379> ttl key5
(integer) 2
192.168.11.50:6379> ttl key5
(integer) 2
192.168.11.50:6379> ttl key5
(integer) 1
192.168.11.50:6379> ttl key5
(integer) -2
192.168.11.50:6379> get key5
(nil)
```

PERSIST 命令去除超时时间

```shell
192.168.11.50:6379> set key5 v5
OK
192.168.11.50:6379> expire key5 100
(integer) 1
192.168.11.50:6379> ttl key5
(integer) 96
192.168.11.50:6379> PERSIST key5
(integer) 1
192.168.11.50:6379> ttl key5
(integer) -1
```

### Redis数据类型之列表操作

LPUSH命令可向list的左边添加一个新元素(头部)

RPUSH命令可向list的右边添加一个新元素(尾部)

LRANGE可以从list中取出一定范围的元素

```shell
192.168.11.50:6379> rpush list1 A
(integer) 1
192.168.11.50:6379> rpush list1 B
(integer) 2
192.168.11.50:6379> lpush list1 first
(integer) 3
192.168.11.50:6379> lrange list1 0 -1
1) "first"
2) "A"
3) "B"
192.168.11.50:6379> lrange list1 1 -1
1) "A"
2) "B"
192.168.11.50:6379> lrange list1 2 -1
1) "B"
```

RPOP删除右边第一个

LPOP删除左边第一个

```shell
192.168.11.50:6379> RPOP list1
"B"
192.168.11.50:6379> lrange list1 0 2
1) "first"
2) "A"
192.168.11.50:6379> LPOP list1
"first"
192.168.11.50:6379> lrange list1 0 2
1) "A"
```

### Redis数据类型之hash操作

HMSET设置多个域

HGET取回单个域

HMGET取回一系列的值

```shell
192.168.11.50:6379> hmset user:1000 username blsnt age 23 job it
OK
192.168.11.50:6379> hget user:1000 username  #取单个
"blsnt"
192.168.11.50:6379> hmget user:1000 username age job  #取多个
1) "blsnt" 
2) "23"
3) "it"
192.168.11.50:6379> HGETALL user:1000
1) "username"
2) "blsnt"
3) "age"
4) "23"
5) "job"
6) "it"
```

### Redis数据类型之集合操作

集合是字符串的无序排列

SADD将新的元素添加到set中

SMEMBERS 查看集合中的内容

```shell
192.168.11.50:6379> sadd set1 1 2 3 4 5 6
(integer) 6
192.168.11.50:6379> SMEMBERS set1
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
6) "6"
```

SDIFF计算集合的差异成员

```shell
192.168.11.50:6379> sadd set1 1 2 3 4
(integer) 4
192.168.11.50:6379> sadd set2 1 4 5
(integer) 3
192.168.11.50:6379> SDIFF set1 set2
1) "2"
2) "3"
```

SINTER计算集合的交集

```shell
192.168.11.50:6379> SINTER set1 set2
1) "1"
2) "4"
```

SUNION计算集合的并集

```shell
192.168.11.50:6379> SUNION set1 set2
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
```

SREM删除指定的值

```shell
192.168.11.50:6379> SMEMBERS set1
1) "1"
2) "2"
3) "3"
4) "4"
192.168.11.50:6379> srem set1 1 2 3
(integer) 3
192.168.11.50:6379> SMEMBERS set1
1) "4"
```

> 集合里面不会出现重复的数据

### Sentine(哨兵)介绍

Redis Sentinel是一个分布式系统，Redis Sentinel为Redis提供高可用性，可以在没有人为干预的情况下阻止某种类型的故障

* Redis的主从模式下，主节点一旦发生故障不能提供服务，需要人工干预，将从节点晋升为主节点，同时还需要修改客户端配置
* Redis从2.8发布了一个稳定版本的Redis Sentinel，当前版本的Sentinel称为Sentinel2，它是使用更强大和简单的预测算法来重写初始Sentinel实现
* Sentinel 架构解决了Redis主从人工干预的问题
* Redis Sentinel是Redis的高可用实现方案，在实际生产环境中，对提高整个系统可用性是非常有帮助的

* 可以在一个架构中运行多个Sentinel进程，这些进程使用流言协议来接收关于主服务器是否下线的信息，并使用投票协议，来决定是否执行自动故障迁移，以及选择哪个从服务器作为新的主服务器
* Redis的Sentinel系统用于管理多个Redis服务器，该系统执行一下三个任务
  * 监控
    * Sentinel会不断的定期检查你的主服务器和从服务器是否运作正常
  * 提醒
    * 当被监控的某个Redis服务器出现问题时，Sentinel可以通过API向管理员或者其他应用程序发送通知
  * 自动故障迁移
    * 当一个主服务器不能正常工作时，Sentinel会开始一次自动故障迁移操作，它会将失效主服务器的其中一个从服务器升级为新的主服务器，并让失效主服务器的其他从服务器改为复制新的主服务器，当新客户端试图连接失效的主服务器时，集群也会向客户端返回新的主服务器的地址，使得集群可以使用新服务器代替失效服务器



### Sentinel(哨兵)和主从的区别

![](http://images.dregs.top/images/20190814165510.png)

### Sentinel(哨兵)安装部署

主机规划

| 角色               | IP            | 端口       |
| ------------------ | ------------- | ---------- |
| Master-Sentinel-01 | 192.168.11.50 | 6379,26379 |
| Slave-Sentinel-02  | 192.168.11.51 | 6379,26379 |
| Slave-Sentinel-03  | 192.168.11.52 | 6379,26379 |

所有节点都需要执行的命令

```shell
mkdir -p /server/tools
mkdir -p /data/redis_cluster/redis_6379
mkdir -p /opt/redis_cluster/redis_6379/{conf,pid,logs}
```

先将master安装好，然后直接从master 将命令cp到两台slave上

 ```shell
cd /server/tools
wget http://download.redis.io/releases/redis-3.2.9.tar.gz
tar zxf redis-3.2.9.tar.gz -C /opt/redis_cluster
ln -s /opt/redis_cluster/redis-3.2.9/ /opt/redis_cluster/redis
make && make install
#将redis拷贝到slave上面
scp -r /opt/redis_cluster/ 192.168.11.51:/opt
scp /usr/local/bin/redis* 192.168.11.51:/usr/local/bin
scp /usr/lib/systemd/system/redis.service 192.168.11.51:/usr/lib/systemd/system/
scp -r /opt/redis_cluster/ 192.168.11.52:/opt
scp /usr/local/bin/redis* 192.168.11.52:/usr/local/bin
scp /usr/lib/systemd/system/redis.service 192.168.11.52:/usr/lib/systemd/system/
 ```

master的配置文件

```shell
vim /opt/redis_cluster/redis_6379/conf/6379.conf
# 以守护进程模式启动
daemonize yes

# 绑定的主机地址
bind 192.168.11.50

# 监听的端口
port 6379

# pid文件和log文件的保存地址
pidfile /opt/redis_cluster/redis_6379/pid/redis_6379.pid
logfile /opt/redis_cluster/redis_6379/logs/redis_6379.log

# 设置数据库的数量，默认数据库为0
databases 16

# 指定本地持久化文件的文件名，默认是dump.rdb
dbfilename redis_6379.rdb

#本地数据库的目录
dir /data/redis_cluster/redis_6379
```

slave的两个配置文件

```shell
vim /opt/redis_cluster/redis_6379/conf/6379.conf
# 以守护进程模式启动
daemonize yes

# 绑定的主机地址
bind 192.168.11.51  #两个slave不一样的地方就是这，修改一下ip

# 监听的端口
port 6379

# pid文件和log文件的保存地址
pidfile /opt/redis_cluster/redis_6379/pid/redis_6379.pid
logfile /opt/redis_cluster/redis_6379/logs/redis_6379.log

# 设置数据库的数量，默认数据库为0
databases 16

# 指定本地持久化文件的文件名，默认是dump.rdb
dbfilename redis_6379.rdb

#主从复制 添加这行即可 主不用动
slaveof 192.168.11.50 6379

#本地数据库的目录
dir /data/redis_cluster/redis_6379
```

> 记得要修改配置文件里面的IP地址

以上主从复制就已经配置完毕

接下来配置哨兵(所有节点都要创建)

```shell
mkdir -p /data/redis_cluster/redis_26379
mkdir -p /opt/redis_cluster/redis_26379/{conf,pid,logs}
```

master配置sentinel

```shell
vim /opt/redis_cluster/redis_26379/conf/redis_26379.conf
bind 192.168.11.50
port 26379
daemonize yes
logfile /opt/redis_cluster/redis_26379/logs/redis_26379.log
dir /data/redis_cluster/redis_26379
sentinel monitor mymaster 192.168.11.50 6379 2
sentinel down-after-milliseconds mymaster 30000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 180000
```

两台slave配置文件

```shell
vim /opt/redis_cluster/redis_26379/conf/redis_26379.conf
bind 192.168.11.51   #修改一下各自的ip即可
port 26379
daemonize yes
logfile /opt/redis_cluster/redis_26379/logs/redis_26379.log
dir /data/redis_cluster/redis_26379
sentinel monitor mymaster 192.168.11.50 6379 2
sentinel down-after-milliseconds mymaster 30000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 180000
```

配置详解

sentinel monitor mymaster 192.168.11.50 6379 2

mymaster主节点别名 主节点ip 端口，判断主节点失败，两个sentinel节点同意

sentinel down-after-milliseconds mymaster 30000

选项指定里 Sentinel认为服务器已经断线所需要的毫秒数

sentinel parallel-syncs mymaster 1

向新的主节点发起复制操作的从节点个数，1轮询发起复制

sentinel failover-timeout mymaster 180000

故障转移超时时间

### **哨兵故障转移**

Redis sentinel存在多个从节点时，如果想要将指定的从节点晋升为主节点，可以将其他从节点的slavepriority配置为0，但是需要注意的是failover后，将slave-priority调回原值

```shell
#查询命令
config get slave-priority
#设置命令
config set slave-priority 0
#生效
sentinel failover
```



### Redis Cluster介绍与安装

#### **Redis Cluster目录规划**

Redis安装目录

```shell
/opt/redis_cluster/redis_{PORT}/{conf,logs,pid}
```

Redis数据目录

```shell
/data/redis_cluster/redis_{PORT}/redis_{PORT}.rdb
```

Redis Cluster拓扑

![](http://images.dregs.top/images/20190822143347.png)

#### **Redis Cluster搭建部署**

创建软件目录和数据目录,三台节点都要操作

```shell
mkdir -p /opt/redis_cluster/redis_{6380,6381}/{conf,logs,pid}
mkdir -p /data/redis_cluster/redis_{6380,6381}
```

 集群配置文件

```shell
vim /opt/redis_cluster/redis_6380/conf/redis_6380.conf
bind 192.168.11.50
port 6380
daemonize yes
pidfile /opt/redis_cluster/redis_6380/pid/redis_6380.pid
logfile /opt/redis_cluster/redis_6380/logs/redis_6380.log
dbfilename redis_6380.rdb
dir /data/redis_cluster/redis_6380/
cluster-enabled yes
cluster-config-file nodes_6380.conf
cluster-node-timeout 15000
```

#### **Redis Cluster快捷部署**

思路：

* 部署一台服务器上的2个集群节点
* 发送完成后修改其他主机的ip地址
* 使用ansible或salt批量部署

```shell
cd /opt/redis_cluster
cp redis_6380/conf/redis_6380.conf redis_6381/conf/redis_6381.conf
sed -i 's#6380#6381#g' redis_6381/conf/redis_6381.conf
```

拷贝到其他节点

```shell
scp -r  /opt/redis_cluster/redis_638* 192.168.11.51:/opt/redis_cluster/
scp -r  /opt/redis_cluster/redis_638* 192.168.11.52:/opt/redis_cluster/
```

将每个节点的Redis启动即可

```shell
redis-server /opt/redis_cluster/redis_6380/conf/redis_6380.conf
redis-server /opt/redis_cluster/redis_6381/conf/redis_6381.conf
```

> 当把每个节点都启动后查看进程会有cluster的字样

![](http://images.dregs.top/images/20190822150405.png)

但是登录后执行`CLUSTER NODES` 命令会发现每个节点自己的ID，目前集群内的节点还没有互相发现，所以搭建的redis集群我们第一步要做的就是让集群内的节点相互发现

![](http://images.dregs.top/images/20190822150756.png)

**Redis CLuster手动配置节点发现**

在执行节点发现命令之前我们先查看一下集群的数据目录会发现有生成集群的配置文件

![](http://images.dregs.top/images/20190822151104.png)

查看之后发现只有自己的节点，等节点全部发现后会把所发现的节点ID写入这个文件

![](http://images.dregs.top/images/20190822151225.png)

> 集群模式的Redis除了原有的配置文件之外又加了一份集群配置文件，当集群内节点信息发生变化，如添加节点，节点下线，故障转移等，节点会自动保存集群状态到配置文件，需要注意的是，Redis自动维护集群配置文件，不需要手动修改，防止节点重启时产生错乱

**节点发现使用`CLUSTER MEET {IP} {PORT}`**

提示：在集群内任意一台机器执行此命令就可以

```shell
 [root@node1 ~]# redis-cli -h 192.168.11.50 -p 6380
192.168.11.50:6380> CLUSTER MEET 192.168.11.50 6381
OK
192.168.11.50:6380> CLUSTER MEET 192.168.11.51 6380
OK
192.168.11.50:6380> CLUSTER MEET 192.168.11.51 6381
OK
192.168.11.50:6380> CLUSTER MEET 192.168.11.52 6380
OK
192.168.11.50:6380> CLUSTER MEET 192.168.11.52 6381
OK
192.168.11.50:6380> CLUSTER NODES
7829e2a4180de48f18a983b9e4bde508446d1775 192.168.11.52:6380 master - 0 1566458327787 4 connected
b5dc7837ad363d459b34f29a2f4e8d74336710a4 192.168.11.51:6380 master - 0 1566458329812 5 connected
a9a65e40131e8cd10f9570843e84fe971b9c79c3 192.168.11.51:6381 master - 0 1566458326268 3 connected
fdf71cf84f70eaf2f63797e7699c2542d676a3df 192.168.11.52:6381 master - 0 1566458330825 0 connected
19e09033f12389c5b5bb952542961a7bc1670e8a 192.168.11.50:6381 master - 0 1566458328800 1 connected
8b3405e7cb3fdce3870582ff7dcd0c6bef93d306 192.168.11.50:6380 myself,master - 0 0 2 connected
```

再次查看集群配置文件

![](http://images.dregs.top/images/20190822152057.png)





#### **Redis CLuster手动分配槽位**

虽然节点之间已经互相发现了，但是此时集群还是不可用的状态，因为并没有给节点分配槽位，而且必须是所有的槽位都分配完毕后整个集群才是可以用的状态

![](http://images.dregs.top/images/20190822152722.png)

我们有6个节点，但是真正写入都只有3个节点，其他3个节点只是作为主节点的从节点，也就是说，只需要分配三个节点的槽位即可

分配槽位的方法：

分配槽位需要在每个主节点上来配置，此时有2种方法执行

1. 分别登录到每个主节点的客户端来执行命令
2. 在其他一台机器上用redis客户端远程登录到其他服务器的主节点上执行命令

每个节点执行命令

```shell
[root@node1 ~]# redis-cli -h 192.168.11.50 -p 6380 cluster addslots {0..5461}
ok
[root@node2 ~]# redis-cli -h 192.168.11.51 -p 6380 cluster addslots {5462..10922}
ok
[root@node3 ~]# redis-cli -h 192.168.11.52 -p 6380 cluster addslots {10923..16383}
ok
```

分配完成所有的槽位以后我们再查看一下集群的节点状态和集群状态

![](http://images.dregs.top/images/20190822155350.png)

#### **Redis Cluster手动配置高可用**

虽然这个时候集群是可用的了，但是整个集群只要有一台机器坏掉了，那么整个集群都是不可用的，所以这个时候需要用到其他三个节点分别作为三个主节点的从节点，以应对集群主几点故障时可以进行自动切换以保证集群高可用

注意：

1. 不要让复制节点复制本机器的主节点，因为如果那样的话机器挂了集群还是不可用状态，所以复制节点要复制其他服务器的主节点
2. 使用redis-trid工具自动分配的时候有概率会出现复制节点和主节点在同一台机器上的情况，需要注意

![](http://images.dregs.top/images/20190822143347.png)

在一台机器上使用redis客户端远程操作集群其他节点

注意：

1. 需要执行命令的是每个从服务器的从节点
2. 注意主从的ID不要搞混了

```shell
[root@node1 ~]# redis-cli -h 192.168.11.50 -p 6381 cluster nodes
[root@node1 ~]# redis-cli -h 192.168.11.50 -p 6381 CLUSTER REPLICATE 7829e2a4180de48f18a983b9e4bde508446d1775
[root@node1 ~]# redis-cli -h 192.168.11.51 -p 6381 CLUSTER REPLICATE 8b3405e7cb3fdce3870582ff7dcd0c6bef93d306
[root@node1 ~]# redis-cli -h 192.168.11.52 -p 6381 CLUSTER REPLICATE b5dc7837ad363d459b34f29a2f4e8d74336710a4
```

![](http://images.dregs.top/images/20190822165715.png)

#### **Redis Cluster测试集群**

测试写一条数据

```shell
[root@node1 ~]# redis-cli -h 192.168.11.50 -p 6380 set key_1 vlaue_1
(error) MOVED 11998 192.168.11.52:6380
```

结果提示error，但是给出了集群另一个节点的地址 ，结果是没有写入的，这是因为使用集群后由于数据被分片了，所以并不是说在哪台机器上写入数据就会在哪台机器的节点上写入，集群的数据都写入和读取涉及到了另外一个概念，ASK路由

**Redis Cluster ASK路由介绍**

在集群模式下，Redis接受任何键相关命令时首先会计算键对应的槽，再根据槽找出所对应的节点，如果节点时自身，则处理键命令，否则回复MOVED重定向错误，通知客户端请求正确的节点，这个过程称为Mover重定向

![](http://images.dregs.top/images/20190822194452.png)

知道了ASK路由后，我们使用-c选项批量插入一些数据

```shell
#!/bin/bash
for i in $(seq 1 1000)
do
  redis-cli -c -h 192.168.11.50 -p 6380 set key_${i} value_${i}
done
```

写入后我们同样使用-c选项来读取刚才插入的键值，然后查看下redis会不会帮我们路由到正确的节点上

![](http://images.dregs.top/images/20190822195718.png)

#### **Redis Cluster模拟故障转移**

我们已经手动的把一个redis高可用的集群部署完毕了，但是还没有模拟过故障，这里我们就模拟故障，挺掉其中一台主机的redis节点，然后查看一下集群的变化

我们停掉192.168.11.52上面的redis集群节点。然后观察状态，理想情况应该是50上面的6381从节点升级为主节点

![](http://images.dregs.top/images/20190822201525.png)

成功升级master

![](http://images.dregs.top/images/20190822201742.png)

虽然我们已经测试了故障切换的功能，但是节点修复后还是需要重新上线的，所有这里测试节点重新上线后的操作

重新启动52的6380，然后观察日志

![](http://images.dregs.top/images/20190822202145.png)

观察50上面的6381日志

![](http://images.dregs.top/images/20190822202304.png)

此时发生了角色转换，52的6380上线后发现自己的槽位被指派给了另一个节点，则以限制集群的配置为准，变为新节点6381的从节点

如果想让修复后的节点重新上线为主节点，可以执行`CLUSTER FAILOVER`命令

```shell
[root@node3 ~]# redis-cli -h 192.168.11.52 -p 6380
192.168.11.52:6380> CLUSTER FAILOVER
OK
```

观察50上的6381日志

![](http://images.dregs.top/images/20190822202810.png)

### Redis Cluster使用Redis-trib.rb搭建cluster

手动搭建集群便于理解集群创建的流程和细节，不过手动搭建集群需要很多步骤，当集群节点众多时，必然会加大搭建集群的复杂度和运维成本，因此官方提供了redis-trib.rb工具方便我们快速搭建集群

redis-trib.rb是采用ruby实现的redis集群管理工具，内部通过Cluster相关命令帮我们简化集群创建，检查，槽迁移和均衡等常见运维操作

使用前安装ruby依赖环境,我们在11.50上进行操作

```shell
[root@node1 ~]# yum -y install rubygems
[root@node1 ~]# gem sources --remove https://rubygems.org/
[root@node1 ~]# gem sources -a http://mirrors.aliyun.com/rubygems/
[root@node1 ~]# gem install redis -v 3.3.5
```

三个节点删除集群数据目录，然后启动就是初始状态。

```shell
rm -rf /data/redis_cluster/redis_6380/*
rm -rf /data/redis_cluster/redis_6381/*
```

三个节点启动起来

```shell
redis-server /opt/redis_cluster/redis_6380/conf/redis_6380.conf
redis-server /opt/redis_cluster/redis_6381/conf/redis_6381.conf
```

进入redis安装目录下的src目录

```shell
cd /opt/redis_cluster/redis/src/
```

执行创建命令

```shell
./redis-trib.rb create --replicas 1 192.168.11.50:6380 192.168.11.51:6380 192.168.11.52:6380 192.168.11.50:6381 192.168.11.51:6381 192.168.11.52:6381
```

输入yes即可

检查集群完整性

```shell
./redis-trib.rb check 192.168.11.50:6380
```

 查看集群节点信息

```shell
redis-cli -h 192.168.11.50 -p 6380 cluster nodes
```

> 默认指定的master可能会有错误，比如一台50的主节点和从节点都在一台机器上，这是不安全的，我们的从应该在不同的机器上面

![](http://images.dregs.top/images/20190822221136.png)根据以上图我们手动调整从节点

通过`CLUSTER REPLICATE`命令调整主从关系

![](http://images.dregs.top/images/20190823112813.png)

### **Redis Cluster工具扩容**

Redis集群的扩容操作可分为以下几个步骤

1. 准备新节点
2. 加入集群
3. 迁移槽和数据

新节点安装配置(我这是在node3直接安装)

```shell
mkdir -p /opt/redis_cluster/redis_{6395,6396}/{conf,logs,pid}
mkdir -p /data/redis_cluster/redis_{6395,6396}

cp /opt/redis_cluster/redis_6380/conf/redis_6380.conf /opt/redis_cluster/redis_6395/conf/redis_6395.conf

cp /opt/redis_cluster/redis_6380/conf/redis_6380.conf /opt/redis_cluster/redis_6396/conf/redis_6396.conf

sed -i 's#6380#6395#g' /opt/redis_cluster/redis_6395/conf/redis_6395.conf
sed -i 's#6380#6396#g' /opt/redis_cluster/redis_6396/conf/redis_6396.conf
```

将节点启动起来

```shell
redis-server /opt/redis_cluster/redis_6395/conf/redis_6395.conf
redis-server /opt/redis_cluster/redis_6396/conf/redis_6396.conf
```

发现节点(任意节点执行都可以)

```shell
 [root@node1 ~]# redis-cli -c -h 192.168.11.50 -p 6380 cluster meet 192.168.11.52 6395
OK
 [root@node1 ~]# redis-cli -c -h 192.168.11.50 -p 6380 cluster meet 192.168.11.52 6396
OK
```

使用脚本工具扩容(在node1执行，因为安装了ruby环境)

```shell
cd /opt/redis_cluster/redis/src
./redis-trib.rb reshard 192.168.11.50:6380 #这个只要是其中一个节点就可以
```

打印出集群每个节点信息后，reshard命令需要确认迁移的槽数量，这里我们输入了4096个

![](http://images.dregs.top/images/20190823123452.png)

```shell
How many slots do you want to move (from 1 to 16384)? 4096
```

输入6395的节点ID作为目标节点，也就是要扩容的节点，目标节点只能指定一个

```shell
What is the receiving node ID? 563c955536214e6e688d5b4807558aeeb6d70475
```

之后输入源节点的ID，这里分别输入每个主节点的6380的ID，最后用done表示结束(可以直接用all)

```shell
Source node #1:61426b3fd7168a45ddac5b9554fd40aa4d1ea68f
Source node #2:4396898272bc71bcdf4285872760c1f84ee70239
Source node #3:656cb3cd94513e649726d9742631acd854041a54
Source node #4:done
```

迁移完成后命令会自动退出，这时候我们查看一下集群的状态

![](http://images.dregs.top/images/20190823124747.png)

最后集群主从关系已经对应上了

通过`CLUSTER REPLICATE`命令调整主从关系

![](http://images.dregs.top/images/20190823133300.png)

### Redis Cluster工具收缩

1. 首先需要确定下线节点是否有负责的槽，如果是，需要把槽迁移到其他节点，保证节点下线后整个集群槽点映射到完整性
2. 当下线节点不再负责槽或者本身是从节点时，就可以通知集群内其他节点忘记下线节点，当所有节点忘记该节点后可以正常关闭

我们将刚才新添加的节点下线，也就是6395和6396

收缩和扩容迁移的方向相反，6395变为源节点，其他节点变为目标节点，源节点把自己负责的4096个槽均匀的迁移到其他节点上

由于redis-trib reshard命令只能有一个目标节点，因此需要执行3次reshard命令

```shell
 [root@node1 /opt/redis_cluster/redis/src]# ./redis-trib.rb reshard 192.168.11.50:6380
 How many slots do you want to move (from 1 to 16384)? 1365
 What is the receiving node ID? 6380ID
 Source node #1:6395ID
 Source node #2:done
 Do you want to proceed with the proposed reshard plan (yes/no)? yes
```

 由于我们的集群做了高可用，所以当主节点下线的时候从节点也会顶上，所以我们最好先下线从节点，然后在下线主节点

```shell
./redis-trib.rb del-node 192.168.11.52:6396 ID
./redis-trib.rb del-node 192.168.11.52:6395 ID
```

### Redis 集群单节点数据导入集群

刚切换到redis集群的时候肯定会面临数据导入问题，这里推荐使用redis-migrate-tool工具来导入

[官网网址](http://www.oschina.net/p/redis-migrate-tool)

[GitHub](https://github.com/vipshop/redis-migrate-tool)

**安装工具**

```shell
cd /opt/redis_cluster
git clone https://github.com/vipshop/redis-migrate-tool.git
cd redis-migrate-tool
autoreconf -fvi
./configure
make 
make install
```

