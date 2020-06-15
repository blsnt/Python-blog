- [环境准备](#%E7%8E%AF%E5%A2%83%E5%87%86%E5%A4%87)
- [SaltStack四大功能](#saltstack%E5%9B%9B%E5%A4%A7%E5%8A%9F%E8%83%BD)
- [SaltStack四种运行模式](#saltstack%E5%9B%9B%E7%A7%8D%E8%BF%90%E8%A1%8C%E6%A8%A1%E5%BC%8F)
- [SaltStack-部署和认证](#saltstack-%E9%83%A8%E7%BD%B2%E5%92%8C%E8%AE%A4%E8%AF%81)
- [SaltStack-远程执行](#saltstack-%E8%BF%9C%E7%A8%8B%E6%89%A7%E8%A1%8C)
- [SaltStack-配置管理](#saltstack-%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86)
  * [SaltStack状态](#saltstack%E7%8A%B6%E6%80%81)
  * [SaltStack高级状态](#saltstack%E9%AB%98%E7%BA%A7%E7%8A%B6%E6%80%81)
- [SaltStack-SSH实战](#saltstack-ssh%E5%AE%9E%E6%88%98)
- [SaltStack-Grains详解](#saltstack-grains%E8%AF%A6%E8%A7%A3)
- [SaltStack-Pillar详解](#saltstack-pillar%E8%AF%A6%E8%A7%A3)
- [SaltStack远程执行-执行模块](#saltstack%E8%BF%9C%E7%A8%8B%E6%89%A7%E8%A1%8C-%E6%89%A7%E8%A1%8C%E6%A8%A1%E5%9D%97)
- [SaltStack远程执行-返回](#saltstack%E8%BF%9C%E7%A8%8B%E6%89%A7%E8%A1%8C-%E8%BF%94%E5%9B%9E)
- [SaltStack配置管理-LAMP状态实现](#saltstack%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86-lamp%E7%8A%B6%E6%80%81%E5%AE%9E%E7%8E%B0)
- [SaltStack-配置管理-jinja模板](#saltstack-%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86-jinja%E6%A8%A1%E6%9D%BF)
- [SaltStack-job管理](#saltstack-job%E7%AE%A1%E7%90%86)
## 环境准备

* 安装操作系统 CentOS-7.x-x86_64。

* 基本系统：1VCPU+2048M 内存+50G（动态）硬盘。

  * 网络选择：使用网络地址转换（NAT）。

  * 软件包选择：Minimal Install。

  * 关闭 iptables 和 SELinux。

* 设置所有节点的主机名和 IP 地址，使用/etc/hosts 做好主机名解析

## SaltStack四大功能

* 远程执行
* 配置管理
* 云管理
* 事件驱动

## SaltStack四种运行模式

* Local
* Master/Minion
* Salt SSH
* Syndic(类似代理)

## SaltStack-部署和认证

[SaltStack 官网部署方式](https://repo.saltstack.com/#rhel)

两台虚拟机都要安装

```shell
yum -y install https://repo.saltstack.com/yum/redhat/salt-repo-latest.el7.noarch.rpm 
```

node1安装master和minion

```shell
[root@node1 ~]# yum -y install salt-master salt-minion
```

node2安装minion

```shell
[root@node2 ~]# yum -y install salt-minion
```

相当于node1为master即为客户端，node2为客户端

> windows不支持master

node1启动master

```shell
[root@node1 ~]# systemctl start salt-master.service
[root@node1 ~]# systemctl enable salt-master.service
```

node1修改minion配置并启动

```shell
[root@node1 ~]# vim /etc/salt/minion
master: 192.168.11.50
id: node1    #如果不修改，默认会用hostname的值
[root@node1 ~]# systemctl start salt-minion
[root@node1 ~]# systemctl enable salt-minion
```

node2修改minion配置并启动

```shell
[root@node2 ~]# vim /etc/salt/minion
master: 192.168.11.50
id: node2
[root@node2 ~]# systemctl start salt-minion
[root@node2 ~]# systemctl enable salt-minion
```

node1同意所有minion请求的key从而**双向通信**

```shell
[root@node1 ~]# salt-key #列出客户端
Accepted Keys:
Denied Keys:
Unaccepted Keys:
node1
node2
Rejected Keys:
[root@node1 ~]# salt-key -a node1   #同意单个客户端
The following keys are going to be accepted:
Unaccepted Keys:
node1
Proceed? [n/Y] y
Key for minion node1 accepted.
[root@node1 ~]# salt-key -A   #同意所有客户端
The following keys are going to be accepted:
Unaccepted Keys:
node2
Proceed? [n/Y] y
Key for minion node2 accepted.
[root@node1 ~]# salt-key   #查看发现已经全部同意
Accepted Keys:
node1
node2
Denied Keys:
Unaccepted Keys:
Rejected Keys:
```

master节点

> /etc/salt/pki/master/minions_pre #放置的是未经master同意的key
>
> /etc/salt/pki/master/minions #放置的是已经通过salt-key同意的key

minion节点(客户端)

> /etc/salt/pki #里面有存放master的公钥

如果修改了minion id怎么办

> 客户端先停止minion，master然后删除使用salt-key -d id名，客户端启动minion，master salt-key同意就可以了

## SaltStack-远程执行

测试命令是否可以执行成功

```shell
[root@node1 ~]# salt '*' test.ping  # 如下代表node和node2都没有问题，*代表所有机器，加单引号转义
node2:
    True
node1:
    True
[root@node1 ~]# salt '*' cmd.run 'df -h' # 获取磁盘使用情况,cmd.run是超级命令
node1:
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/sda2        19G  1.6G   18G   9% /
    devtmpfs        479M     0  479M   0% /dev
    tmpfs           489M   28K  489M   1% /dev/shm
    tmpfs           489M  6.8M  482M   2% /run
    tmpfs           489M     0  489M   0% /sys/fs/cgroup
    /dev/sda1      1014M  120M  895M  12% /boot
    tmpfs            98M     0   98M   0% /run/user/0
node2:
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/sda2        19G  1.6G   18G   9% /
    devtmpfs        479M     0  479M   0% /dev
    tmpfs           489M   12K  489M   1% /dev/shm
    tmpfs           489M  6.8M  482M   2% /run
    tmpfs           489M     0  489M   0% /sys/fs/cgroup
    /dev/sda1      1014M  120M  895M  12% /boot
    tmpfs            98M     0   98M   0% /run/user/0
```

## SaltStack-配置管理

### SaltStack状态

状态管理->神笔马良

描述状态->SaltStack实现

> 已经执行过的就不会再执行

YAML编写规范

* 缩进
  * YAML使用一个固定的缩进风格表示数据层结构关系，Salt 需要每个缩进级别由两个空格组成
  * 不要使用talbe
* 冒号
  * 每个key后面有一个冒号，冒号后面有个空格 
* 短横线
  * 想要表示列表项，使用一个短横杠加一个空格，多个项使用同样的缩进级别作为同一列表的一部分

修改master 状态描述文件存放位置

```shell
[root@node1 ~]# vim /etc/salt/master
file_roots:
  base:
    - /srv/salt
[root@node1 ~]# mkdir /srv/salt/web -p
[root@node1 ~]# systemctl restart salt-master
```

写一个apache安装与启动的状态描述文件

```shell
[root@node1 ~]# cd /srv/salt/web
[root@node1 web]# vim apache.sls
apache-install:
  pkg.installed:
    - name: httpd

apache-service:
  service.running:
    - name: httpd
    - enable: True
    
#使用node2执行这个状态
[root@node1 web]# salt 'node2' state.sls web.apache
node2:
----------
          ID: apache-install
    Function: pkg.installed
        Name: httpd
      Result: True
     Comment: The following packages were installed/updated: httpd
     Started: 08:19:42.127389
    Duration: 10713.988 ms
     Changes:
              ----------
              httpd:
                  ----------
                  new:
                      2.4.6-89.el7.centos
                  old:
              httpd-tools:
                  ----------
                  new:
                      2.4.6-89.el7.centos
                  old:
              mailcap:
                  ----------
                  new:
                      2.1.41-2.el7
                  old:
----------
          ID: apache-service
    Function: service.running
        Name: httpd
      Result: True
     Comment: Service httpd has been enabled, and is running
     Started: 08:19:53.738830


    Duration: 233.833 ms
     Changes:
              ----------
              httpd:
                  True

Summary for node2
------------
Succeeded: 2 (changed=2)
Failed:    0
------------
Total states run:     2
Total run time:  10.948 s
```

### SaltStack高级状态

```shell
[root@node1 ~]# cd /srv/salt/
[root@node1 salt]# vim top.sls  #定义所有的客户端需要做的事情
base:
  'node1':
    - web.apache
  'node2':
    - web.apache
[root@node1 salt]# salt '*' state.highstate #*代表所有客户端来查看top.sls并找到自己要做的事情。写单个客户端比如node1，node2 那么就是node1会来查看top.sls文件查看里面有没有自己需要执行的任务
```

## SaltStack-SSH实战

master安装salt-ssh客户端不用安装，实现无agent

```shell
[root@node1 ~]# yum -y install salt-ssh
[root@node1 ~]# vim /etc/salt/roster
#web2:
#  host: 192.168.42.2
node1:
  host: 192.168.11.50
  user: root
  passwd: 666666

node2:
  host: 192.168.11.51
  user: root
  passwd: 666666
  
[root@node1 ~]# salt-ssh '*' test.ping -i  #测试，使用-i免交互输入yes
node2:
    True
node1:
    True

[root@node1 ~]# salt-ssh '*' -r 'ifconfig' #执行原生shell命令
[root@node1 ~]# salt-ssh '*' cmd.run 'w'   #执行模块使用命令
[root@node1 ~]# salt-ssh '*' state.sls web.apache  #执行状态
```

## SaltStack-Grains详解

* Minion启动时收集(静态数据)

* Grains应用场景

  * Grains 可以在state系统中使用，用于配置管理模块
  * Grains 可以在target中使用，在用来匹配Minion，比如匹配操作系统，使用-G选项

  * Grains 可以用于信息查询，Grains保存着收集到的客户端的详细信息

常规操作

```shell
[root@node1 ~]# salt 'node1' grains.ls #查看保存的系统信息key
[root@node1 ~]# salt 'node1' grains.items #查看所有信息的值
[root@node1 ~]# salt 'node1' grains.item saltversion #查看值
[root@node1 ~]# salt 'node1' grains.get saltversion  #查看值
[root@node1 ~]# salt 'node1' grains.get ip4_interfaces:eth0 #查看eth0的ip
```

自定义Grains

```shell
[root@node1 ~]# vim /etc/salt/grains
test-grains: test-grains-value
[root@node1 ~]# salt '*' saltutil.sync_grains  # 不重启minion生效
node2:
node1:
[root@node1 ~]# salt '*' grains.item test-grains
node1:
    ----------
    test-grains:
        test-grains-value
node2:
    ----------
```

使用Grains过滤

```shell
[root@node1 ~]# salt -G 'os:CentOS' cmd.run 'uptime'  # 过滤有CentOS的机器执行uptime
node1:
     13:36:54 up  5:05,  1 user,  load average: 0.01, 0.04, 0.05
node2:
     13:36:54 up  5:11,  1 user,  load average: 0.01, 0.04, 0.05
```

使用top.sls来写Grains

```shell
[root@node1 ~]# vim /srv/salt/top.sls
base:
  'os:CentOS':
    - match: grain
    - web.apache
[root@node1 ~]# salt '*' state.sls web.apache 
```

## SaltStack-Pillar详解

Salt 0.9.8 版本增加了pillar(动态数据)

存储位置：存储在master端，存放需要提供给minion的信息

应用场景：敏感信息，每个minion只能访问master分配给自己的pillar

修改pillar存储位置

```shell
[root@node1 ~]# vim  /etc/salt/master
pillar_roots:
  base:
    - /srv/pillar
[root@node1 ~]# mkdir /srv/pillar -p
[root@node1 ~]# systemctl restart salt-master
```

pillar配合grains

```shell
[root@node1 ~]# cd /srv/pillar/
[root@node1 pillar]# vim top.sls   #指定给哪个客户端用，apache指定apache.sls
base:
  'node2':
    - apache
[root@node1 pillar]# vim apache.sls  
{% if grains['os'] == 'CentOS' %}
apache: httpd
{% elif grains['os'] == 'Debian' %}
apache: apache2
{% endif %}
[root@node1 pillar]# salt '*' pillar.items  #node1是没有指定的，所以只有node2
node1:
    ----------
node2:
    ----------
    apache:
        httpd

# 配合grains
[root@node1 ~]# vim /srv/salt/web/apache.sls  #指定pillar下面的apache.sls
apache-install:
  pkg.installed:
    - name: {{ pillar['apache'] }}  

apache-service:
  service.running:
    - name: {{ pillar['apache'] }}
    - enable: True
[root@node1 pillar]# salt '*' state.highstate  #如果报错如下，则是pillar下的top.sls没有添加node1
node1:
    Data failed to compile:
----------
    Rendering SLS 'base:web.apache' failed: Jinja variable 'salt.utils.context.NamespacedDictWrapper object' has no attribute 'apache'
node2:
----------
          ID: apache-install
    Function: pkg.installed
        Name: httpd
      Result: True
     Comment: All specified packages are already installed
     Started: 14:27:12.560070
    Duration: 726.908 ms
     Changes:
----------
          ID: apache-service
    Function: service.running
        Name: httpd
      Result: True
     Comment: The service httpd is already running

▽
base:
     Started: 14:27:13.287826
    Duration: 26.24 ms
     Changes:

Summary for node2
------------
Succeeded: 2
Failed:    0
------------
Total states run:     2
Total run time: 753.148 ms
ERROR: Minions returned with non-zero exit code

#修改为node*代表匹配包括node1和node2都会匹配到，再执行就不会报错了
[root@node1 ~]# cat /srv/pillar/top.sls
base:
  'node*':
    - apache
```

|        | 存储位置 | 类型 | 采集方式               | 场景               |
| ------ | -------- | ---- | ---------------------- | ------------------ |
| Grains | minion   | 静态 | minion启动时，可以刷新 | 获取信息，匹配     |
| Pillar | master   | 动态 | 指定，实时生效         | 匹配，敏感数据配置 |

## SaltStack远程执行-执行模块

[官网模块](https://docs.saltstack.com/en/latest/ref/modules/all/index.html#all-salt-modules)

* service

  * get_all 列出所有正在运行的服务

  ```shell
  [root@node1 ~]# salt 'node2' service.get_all |grep sshd
  ```

  * Status 查看服务状态

  ```shell
  [root@node1 ~]# salt 'node2' service.status sshd
  ```

* state 状态模块

  * show_top 查看minion有哪些状态

* salt-cp 拷贝文件

```shell
[root@node1 ~]# salt-cp '*' /etc/passwd /tmp/passwd  #拷贝文件到所有客户端
node1:
    ----------
    /tmp/passwd:
        True
node2:
    ----------
    /tmp/passwd:
        True 
[root@node1 ~]# salt '*' cmd.run 'head -1 /tmp/passwd'   #查看客户端是否拷贝成功
node1:
    root:x:0:0:root:/root:/bin/bash
node2:
    root:x:0:0:root:/root:/bin/bash
```

## SaltStack远程执行-返回

**所有minion安装mysql-python模块**

```shell
[root@node1 ~]# salt '*' cmd.run 'yum install -y MySQL-python'
[root@node1 ~]# salt '*' pkg.install MySQL-python  #这样也可以
[root@node1 ~]# yum install -y mariadb-server
[root@node1 ~]# systemctl start mariadb
[root@node1 ~]# mysql
MariaDB [(none)]> grant all on salt.* to salt@'%' identified by 'salt';
```

执行sql语句

```shell
CREATE DATABASE  `salt`
  DEFAULT CHARACTER SET utf8
  DEFAULT COLLATE utf8_general_ci;
 
USE `salt`;
 
--
-- Table structure for table `jids`
--
 
DROP TABLE IF EXISTS `jids`;
CREATE TABLE `jids` (
  `jid` varchar(255) NOT NULL,
  `load` mediumtext NOT NULL,
  UNIQUE KEY `jid` (`jid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
#CREATE INDEX jid ON jids(jid) USING BTREE;
 
--
-- Table structure for table `salt_returns`
--
 
DROP TABLE IF EXISTS `salt_returns`;
CREATE TABLE `salt_returns` (
  `fun` varchar(50) NOT NULL,
  `jid` varchar(255) NOT NULL,
  `return` mediumtext NOT NULL,
  `id` varchar(255) NOT NULL,
  `success` varchar(10) NOT NULL,
  `full_ret` mediumtext NOT NULL,
  `alter_time` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  KEY `id` (`id`),
  KEY `jid` (`jid`),
  KEY `fun` (`fun`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
 
--
-- Table structure for table `salt_events`
--
 
DROP TABLE IF EXISTS `salt_events`;
CREATE TABLE `salt_events` (
`id` BIGINT NOT NULL AUTO_INCREMENT,
`tag` varchar(255) NOT NULL,
`data` mediumtext NOT NULL,
`alter_time` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
`master_id` varchar(255) NOT NULL,
PRIMARY KEY (`id`),
KEY `tag` (`tag`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

**minion更改配置文件，连接数据库**

## SaltStack配置管理-LAMP状态实现

```shell
[root@node1 prod]# tree
.
├── apache
│   ├── files  #这里面都是配置文件
│   │   └── httpd.conf
│   └── init.sls
├── mysql
│   ├── files
│   │   └── my.cnf
│   └── init.sls
└── php
    ├── files
    │   └── php.ini
    └── init.sls
[root@node1 prod]# cat apache/init.sls
apache-install:
  pkg.installed:
    - name: httpd

apache-config:
  file.managed:
    - name: /etc/httpd/conf/httpd.conf
    - source: salt://apache/files/httpd.conf
    - user: root
    - group: root
    - mode: 644

apache-service:
  service.running:
    - name: httpd
    - enable: True
[root@node1 prod]# cat php/init.sls
php-install:
  pkg.installed:
    - pkgs:
      - php
      - php-pdo
      - php-mysql

php-config:
  file.managed:
    - name: /etc/php.ini
    - source: salt://php/files/php.ini
    - user: root
    - group: root
    - mode: 644
[root@node1 prod]# cat mysql/init.sls
mysql-install:
  pkg.installed:
    - pkgs:
      - mariadb
      - mariadb-server

mysql-config:
  file.managed:
    - name: /etc/my.cnf
    - source: salt://mysql/files/my.cnf
    - user: root
    - group: root
    - mode: 644

mysql-service:
   service.running:
    - name: mariadb
    - enable: True
[root@node1 salt]# cat /srv/salt/top.sls
base:
  'os:CentOS':
    - match: grain
    - web.apache
prod:
  'node*':
    - mysql.init
    - php.init
    - apache.init
[root@node1 salt]# salt '*' state.highstate
```

**saltstack include和扩展**

```shell
[root@node1 prod]# cat ../top.sls  #top文件定义lamp
base:
  'os:CentOS':
    - match: grain
    - web.apache
prod:
  'node*':
    - lamp
[root@node1 prod]# cat lamp.sls  #调用include并添加扩展
include:
  - apache.init
  - php.init
  - mysql.init

extend:
  php-install:
    pkg.installed:
      - name: php-mbstring
[root@node1 salt]# salt '*' state.highstate
```

**SaltStack require**(我依赖谁)

```shell
[root@node1 apache]# cat init.sls
apache-install:
  pkg.installed:
    - name: httpd

apache-config:
  file.managed:
    - name: /etc/httpd/conf/httpd.conf
    - source: salt://apache/files/httpd.conf
    - user: root
    - group: root
    - mode: 644

apache-service:
  service.running:
    - name: httpd
    - enable: True
    - require:             #固定字段
      - pkg: apache-install  #如果安装失败也不会启动apache
      - file: apache-config  #如果这个文件不存在，apache服务就不启动
[root@node1 salt]# salt '*' state.highstate
```

**SaltStack watch**

```shell
[root@node1 apache]# cat init.sls
apache-install:
  pkg.installed:
    - name: httpd

apache-config:
  file.managed:
    - name: /etc/httpd/conf/httpd.conf
    - source: salt://apache/files/httpd.conf
    - user: root
    - group: root
    - mode: 644

apache-service:
  service.running:
    - name: httpd
    - enable: True
    - reload: True.  #如果添加这个表示重载，不加表示重启
    - watch:         #如果apache的配置文件发生改变我就重启apache
      - file: apache-config
[root@node1 salt]# salt '*' state.highstate
```

SaltStakck

```shell
[root@node1 apache]# cat init.sls
apache-install:
  pkg.installed:
    - name: httpd

apache-config:
  file.managed:
    - name: /etc/httpd/conf/httpd.conf
    - source: salt://apache/files/httpd.conf
    - user: root
    - group: root
    - mode: 644

apache-auth:                    #配置访问apache需要用户认证
  pkg.installed:
    - name: httpd-tools
  cmd.run:
    - name: htpasswd -bc /etc/httpd/conf/htpasswd_file admin admin
    - unless: test -f /etc/httpd/conf/htpasswd_file  #如果已经有上面这个文件就不执行，如果没有则执行

apache-service:
  service.running:
    - name: httpd
    - enable: True
    - watch:
      - file: apache-config
#需要自己在files里面的httpd.conf添加配置,admin目录自己创建，添加index.html
<Directory "/var/www/html/admin">
        AllowOverride ALL
        Order allow,deny
        Allow from all
        AuthType Basic
        AuthName 'hehe'
        AuthUserFile /etc/httpd/conf/htpasswd_file
        Require user admin
</Directory>
[root@node1 apache]# salt 'node1' state.highstate
```

![](http://images.dregs.top/images/20190801152703.png)

## SaltStack-配置管理-jinja模板

```shell
[root@node1 apache]# cat init.sls
apache-install:
  pkg.installed:
    - name: httpd

apache-config:
  file.managed:
    - name: /etc/httpd/conf/httpd.conf
    - source: salt://apache/files/httpd.conf
    - user: root
    - group: root
    - mode: 644
    - template: jinja    #固定字段
    - defaults:          #将变量传入到上面的httpd.conf配置文件中
      PORT: 80           
      IPADDR: {{ grains['fqdn_ip4'][0] }}  #从grains中获取到每台机器的ip，ip不能固定写死

apache-auth:
  pkg.installed:
    - name: httpd-tools
  cmd.run:
    - name: htpasswd -bc /etc/httpd/conf/htpasswd_file admin admin
    - unless: test -f /etc/httpd/conf/htpasswd_file

apache-service:
  service.running:
    - name: httpd
    - enable: True
    - watch:
      - file: apache-config
[root@node1 apache]# cat files/httpd.conf #httpd.conf 里面定义的变量
Listen {{ IPADDR }}:{{ PORT }}
[root@node1 apache]# salt '*' state.highstate
```

## SaltStack-job管理

```shell
[root@node1 ~]# salt '*' saltutil.running #查看正在运行的job
```

salt需求

* 系统初始化

```shell
1.关闭selinux
2.关闭默认iptables
3.时间同步
4.内核调优
5.SSH服务优化
6.文件描述符
7.精简开机系统服务
8.DNS解析
9.字符集
10.hosts文件统一
11.历史记录优化history
12.设置终端超时时间
13.配置yum源
14.基础用户，用户审计
15.常用基础命令
16.
```

