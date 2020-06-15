- [Docker是什么？](#docker%E6%98%AF%E4%BB%80%E4%B9%88)
- [为什么要使用 Docker?](#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E4%BD%BF%E7%94%A8-docker)
- [为什么要使用 Docker？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E4%BD%BF%E7%94%A8-docker)
  * [更高效的利用系统资源](#%E6%9B%B4%E9%AB%98%E6%95%88%E7%9A%84%E5%88%A9%E7%94%A8%E7%B3%BB%E7%BB%9F%E8%B5%84%E6%BA%90)
  * [更快速的启动时间](#%E6%9B%B4%E5%BF%AB%E9%80%9F%E7%9A%84%E5%90%AF%E5%8A%A8%E6%97%B6%E9%97%B4)
  * [一致的运行环境](#%E4%B8%80%E8%87%B4%E7%9A%84%E8%BF%90%E8%A1%8C%E7%8E%AF%E5%A2%83)
  * [持续交付和部署](#%E6%8C%81%E7%BB%AD%E4%BA%A4%E4%BB%98%E5%92%8C%E9%83%A8%E7%BD%B2)
  * [更轻松的迁移](#%E6%9B%B4%E8%BD%BB%E6%9D%BE%E7%9A%84%E8%BF%81%E7%A7%BB)
  * [更轻松的维护和扩展](#%E6%9B%B4%E8%BD%BB%E6%9D%BE%E7%9A%84%E7%BB%B4%E6%8A%A4%E5%92%8C%E6%89%A9%E5%B1%95)
  * [对比传统虚拟机总结](#%E5%AF%B9%E6%AF%94%E4%BC%A0%E7%BB%9F%E8%99%9A%E6%8B%9F%E6%9C%BA%E6%80%BB%E7%BB%93)
- [**CentOS7.x**安装Docker](#centos7x%E5%AE%89%E8%A3%85docker)
- [镜像管理](#%E9%95%9C%E5%83%8F%E7%AE%A1%E7%90%86)
  * [镜像是什么？](#%E9%95%9C%E5%83%8F%E6%98%AF%E4%BB%80%E4%B9%88)
  * [**镜像从哪里来?**](#%E9%95%9C%E5%83%8F%E4%BB%8E%E5%93%AA%E9%87%8C%E6%9D%A5)
  * [**镜像与容器联系**](#%E9%95%9C%E5%83%8F%E4%B8%8E%E5%AE%B9%E5%99%A8%E8%81%94%E7%B3%BB)
  * [**管理镜像常用命令**](#%E7%AE%A1%E7%90%86%E9%95%9C%E5%83%8F%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)
- [容器管理](#%E5%AE%B9%E5%99%A8%E7%AE%A1%E7%90%86)
  * [创建容器常用选项](#%E5%88%9B%E5%BB%BA%E5%AE%B9%E5%99%A8%E5%B8%B8%E7%94%A8%E9%80%89%E9%A1%B9)
  * [容器资源限制](#%E5%AE%B9%E5%99%A8%E8%B5%84%E6%BA%90%E9%99%90%E5%88%B6)
  * [管理容器常用命令](#%E7%AE%A1%E7%90%86%E5%AE%B9%E5%99%A8%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)
- [管理应用程序数据](#%E7%AE%A1%E7%90%86%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E6%95%B0%E6%8D%AE)
  * [将数据从宿主机挂载到容器中的三种方式](#%E5%B0%86%E6%95%B0%E6%8D%AE%E4%BB%8E%E5%AE%BF%E4%B8%BB%E6%9C%BA%E6%8C%82%E8%BD%BD%E5%88%B0%E5%AE%B9%E5%99%A8%E4%B8%AD%E7%9A%84%E4%B8%89%E7%A7%8D%E6%96%B9%E5%BC%8F)
- [Dockerfile](#dockerfile)
  * [Dockerfile指令](#dockerfile%E6%8C%87%E4%BB%A4)
  * [构建业务基础镜像](#%E6%9E%84%E5%BB%BA%E4%B8%9A%E5%8A%A1%E5%9F%BA%E7%A1%80%E9%95%9C%E5%83%8F)
    + [构建Nginx基础镜像](#%E6%9E%84%E5%BB%BAnginx%E5%9F%BA%E7%A1%80%E9%95%9C%E5%83%8F)
    + [构建PHP基础镜像](#%E6%9E%84%E5%BB%BAphp%E5%9F%BA%E7%A1%80%E9%95%9C%E5%83%8F)
    + [构建Tomcat基础镜像](#%E6%9E%84%E5%BB%BAtomcat%E5%9F%BA%E7%A1%80%E9%95%9C%E5%83%8F)
    + [快速部署LNMP网站平台](#%E5%BF%AB%E9%80%9F%E9%83%A8%E7%BD%B2lnmp%E7%BD%91%E7%AB%99%E5%B9%B3%E5%8F%B0)
- [企业级镜像仓库Harbor](#%E4%BC%81%E4%B8%9A%E7%BA%A7%E9%95%9C%E5%83%8F%E4%BB%93%E5%BA%93harbor)
  * [harbor概述](#harbor%E6%A6%82%E8%BF%B0)
  * [Harbor部署](#harbor%E9%83%A8%E7%BD%B2)
  * [基本使用](#%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
- [**Jenkins与Docker的自动化CI/CD流水线实战**](#jenkins%E4%B8%8Edocker%E7%9A%84%E8%87%AA%E5%8A%A8%E5%8C%96cicd%E6%B5%81%E6%B0%B4%E7%BA%BF%E5%AE%9E%E6%88%98)
## Docker是什么？

Docker 最初是 dotCloud 公司创始人 Solomon Hykes 在法国期间发起的一个公司内部项目，它是基于 dotCloud 公司多年云服务技术的一次革新，并于 [2013 年 3 月以 Apache 2.0 授权协议开源](https://en.wikipedia.org/wiki/Docker_(software))，主要项目代码在 [GitHub](https://github.com/moby/moby) 上进行维护。Docker 项目后来还加入了 Linux 基金会，并成立推动 [开放容器联盟（OCI）](https://www.opencontainers.org/)。

Docker 自开源后受到广泛的关注和讨论，至今其 [GitHub 项目](https://github.com/moby/moby) 已经超过 5 万 2 千个星标和一万多个 fork。甚至由于 Docker 项目的火爆，在 2013 年底，[dotCloud 公司决定改名为 Docker](https://blog.docker.com/2013/10/dotcloud-is-becoming-docker-inc/)。Docker 最初是在 Ubuntu 12.04 上开发实现的；Red Hat 则从 RHEL 6.5 开始对 Docker 进行支持；Google 也在其 PaaS 产品中广泛应用 Docker。

Docker 使用 Google 公司推出的 [Go 语言](https://golang.org/) 进行开发实现，基于 Linux 内核的 [cgroup](https://zh.wikipedia.org/wiki/Cgroups)，[namespace](https://en.wikipedia.org/wiki/Linux_namespaces)，以及[AUFS](https://en.wikipedia.org/wiki/Aufs) 类的 [Union FS](https://en.wikipedia.org/wiki/Union_mount) 等技术，对进程进行封装隔离，属于 [操作系统层面的虚拟化技术](https://en.wikipedia.org/wiki/Operating-system-level_virtualization)。由于隔离的进程独立于宿主和其它的隔离的进程，因此也称其为容器。最初实现是基于 [LXC](https://linuxcontainers.org/lxc/introduction/)，从 0.7 版本以后开始去除 LXC，转而使用自行开发的 [libcontainer](https://github.com/docker/libcontainer)，从 1.11 开始，则进一步演进为使用 [runC](https://github.com/opencontainers/runc) 和 [containerd](https://github.com/containerd/containerd)。



![](http://images.dregs.top/images/20190726005246.png)

>`runc` 是一个 Linux 命令行工具，用于根据 [OCI容器运行时规范](https://github.com/opencontainers/runtime-spec) 创建和运行容器。
>
>`containerd` 是一个守护程序，它管理容器生命周期，提供了在一个节点上执行容器和管理镜像的最小功能集。

Docker 在容器的基础上，进行了进一步的封装，从文件系统、网络互联到进程隔离等等，极大的简化了容器的创建和维护。使得 Docker 技术比虚拟机技术更为轻便、快捷。

下面的图片比较了 Docker 和传统虚拟化方式的不同之处。传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程；而容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便。

![](http://images.dregs.top/images/20190726005426.png)

​																		*图 1.4.1.2 - 传统虚拟化*

![](http://images.dregs.top/images/20190726005512.png)

​																		*图 1.4.1.3 - Docker*



## 为什么要使用 Docker?

- 更高效的利用系统资源
- 更快速的启动时间
- 一致的运行环境
- 持续交付和部署
- 更轻松的迁移
- 更轻松的维护和扩展
- 对比传统虚拟机总结

* 使用最广泛的开源容器引擎

## 为什么要使用 Docker？

作为一种新兴的虚拟化方式，Docker 跟传统的虚拟化方式相比具有众多的优势。

### 更高效的利用系统资源

由于容器不需要进行硬件虚拟以及运行完整操作系统等额外开销，Docker 对系统资源的利用率更高。无论是应用执行速度、内存损耗或者文件存储速度，都要比传统虚拟机技术更高效。因此，相比虚拟机技术，一个相同配置的主机，往往可以运行更多数量的应用。

### 更快速的启动时间

传统的虚拟机技术启动应用服务往往需要数分钟，而 Docker 容器应用，由于直接运行于宿主内核，无需启动完整的操作系统，因此可以做到秒级、甚至毫秒级的启动时间。大大的节约了开发、测试、部署的时间。

### 一致的运行环境

开发过程中一个常见的问题是环境一致性问题。由于开发环境、测试环境、生产环境不一致，导致有些 bug 并未在开发过程中被发现。而 Docker 的镜像提供了除内核外完整的运行时环境，确保了应用运行环境一致性，从而不会再出现 *「这段代码在我机器上没问题啊」* 这类问题。

### 持续交付和部署

对开发和运维`DevOps`人员来说，最希望的就是一次创建或配置，可以在任意地方正常运行。

使用 Docker 可以通过定制应用镜像来实现持续集成、持续交付、部署。开发人员可以通过 `Dockerfile` 来进行镜像构建，并结合 `持续集成(Continuous Integration)` 系统进行集成测试，而运维人员则可以直接在生产环境中快速部署该镜像，甚至结合 `持续部署(Continuous Delivery/Deployment)` 系统进行自动部署。

而且使用 `Dockerfile` 使镜像构建透明化，不仅仅开发团队可以理解应用运行环境，也方便运维团队理解应用运行所需条件，帮助更好的生产环境中部署该镜像。

### 更轻松的迁移

由于 Docker 确保了执行环境的一致性，使得应用的迁移更加容易。Docker 可以在很多平台上运行，无论是物理机、虚拟机、公有云、私有云，甚至是笔记本，其运行结果是一致的。因此用户可以很轻易的将在一个平台上运行的应用，迁移到另一个平台上，而不用担心运行环境的变化导致应用无法正常运行的情况。

### 更轻松的维护和扩展

Docker 使用的分层存储以及镜像的技术，使得应用重复部分的复用更为容易，也使得应用的维护更新更加简单，基于基础镜像进一步扩展镜像也变得非常简单。此外，Docker 团队同各个开源项目团队一起维护了一大批高质量的 [官方镜像](https://hub.docker.com/search/?type=image&image_filter=official)，既可以直接在生产环境使用，又可以作为基础进一步定制，大大的降低了应用服务的镜像制作成本。

### 对比传统虚拟机总结

| 特性       | 容器               | 虚拟机      |
| :--------- | :----------------- | :---------- |
| 启动       | 秒级               | 分钟级      |
| 硬盘使用   | 一般为 `MB`        | 一般为 `GB` |
| 性能       | 接近原生           | 弱于        |
| 系统支持量 | 单机支持上千个容器 | 一般几十个  |

## **CentOS7.x**安装Docker

* 系统环境

```shell
[root@blsnt ~]# cat /etc/redhat-release
CentOS Linux release 7.4.1708 (Core)
[root@blsnt ~]# uname -r
3.10.0-693.el7.x86_64
[root@blsnt ~]# systemctl status firewalld.service
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(1)
Hint: Some lines were ellipsized, use -l to show in full.
[root@blsnt ~]# getenforce
Permissive
```

* 安装依赖包

 ```shell
yum install -y yum-utils device-mapper-persistent-data lvm2
 ```

* 添加Docker软件包源

```shell
yum-config-manager \
--add-repo \
https://download.docker.com/linux/centos/docker-ce.repo
```

> 如果网速比较慢可切换到阿里云或者清华源

```shell
#阿里云
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
#清华
wget -O /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo
sed -i 's+download.docker.com+mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo
```

* 安装Docker CE

```shell
yum install -y docker-ce
```

* 启动Docker服务并设置开机启动 

```shell
systemctl start docker 
systemctl enable docker 
```

* docker镜像加速

```shell
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://f1361db2.m.daocloud.io

#重启docker
systemctl restart docker
```

> 官方文档：https://docs.docker.com/

## Docker常用命令

```python
# 查看版本信息
docker version
# 查看命令帮助
docker --help
# 列出镜像
docker images 
docker images ls
# 拉取镜像
docker pull centos:7
# 查找镜像
docker search NAME
# 删除镜像
docker rmi NAME
# 查看容器
docker ps # 查看正在运行的容器
docker ps -a # 查看所有容器
docker ps -l # 查看最后一次运行的容器
# 退出容器
exit
# 交互式容器exit退出后容器自动关闭
docker run -it --name [NAME] IMAGE /bin/bash
# 创建守护式容器
docker run -itd --name NAME IMAGE /bin/bash
# 进入容器
docker exec -it [CONTAINER ID] /bin/bash
# 启动已经停止的容器
docker start [CONTAINER ID]
# 停止容器
docker stop [CONTAINER ID]
# 重启容器
docker restart [CONTAINER ID]
# 删除容器
docker rm -f [CONTAINER ID]
# 删除全部停止的容器
docker rm `docker ps -a -q`
# 查看容器详细信息
docker inspect [CONTAINER NAME]
# 从宿主机（source）拷贝至容器内（target）：
docker cp ./1.txt container1:/root
# 从容器内（source）拷贝至宿主机（target）：
docker cp container1:/root ./1.txt
# 创建容器时指定-p参数将容器内端口映射到宿主机的端口。
docker run -itd --name=NAME -p 宿主机端口:容器端口 IMAGE /bin/bash
# 创建容器时指定-v参数进行目录挂载。
docker run -itd --name=NAME -v 宿主机目录:容器目录 IMAGE /bin/bash
# 镜像打包
docker save -o [dir/NAME.tar] IMAGE_NAME
# 导入打包镜像
docker load -i /root/dir/NAME.tar
```

## 镜像管理

### 镜像是什么？

* 一个分层存储的文件
* 一个软件的环境

* 一个镜像可以创建N个容器

* 一种标准化的交付(任何地方都可以执行这个镜像)

* 一个不包含Linux内核而又精简的Linux操作系统
   镜像不是一个单一的文件，而是有多层构成。我们可以通过docker history <ID/NAME> 查看镜像中各层内容及大小，每层 对应着Dockerfile中的一条指令。Docker镜像默认存储在/var/lib/docker/\<storage-driver\>中。 

### **镜像从哪里来?** 

Docker Hub是由Docker公司负责维护的公共注册中心，包含大量的容器镜像，Docker工具默认从这个公共镜像库下载镜像。 地址:https://hub.docker.com/explore 

### **镜像与容器联系**

![](http://images.dregs.top/images/20190725213858.png)

如图，容器其实是在镜像的最上面加了一层读写层，在运行容器里文件改动时，会先从镜像里要写的文件复制到容器自己的文件系统中(读写层)。如果容器删除了，最上面的读写层也就删除了，改动也就丢失了。所以无论多少个容器共享一个镜像，所做的写操作都是从镜像的文件系统中复制过来操作的，并不会修改镜像的源文件，这种方式提高磁盘利用率。

若想持久化这些改动，可以通过docker commit 将容器保存成一个新镜像

### **管理镜像常用命令**

| 指令    | 描述                                | 示例                                     |
| ------- | ----------------------------------- | ---------------------------------------- |
| ls      | 列出镜像                            | docker image ls 或者 docker images       |
| build   | 构建镜像来自Dockerfile              | docker build -t nginx:v3 .               |
| history | 查看镜像历史                        | docker history tomcat:v1 查看构建历史    |
| inspect | 显示一个或多个镜像详细信息          | docker inspect tomcat:v1                 |
| pull    | 从镜像仓库拉去镜像                  | docker pull mysql:5.7 默认不加版本是最新 |
| push    | 推送一个镜像到镜像仓库              |                                          |
| rm      | 移除一个或多个镜像                  | docker image rm nginx 删除最新nginx镜像  |
| prune   | 移除未使用的镜像，没有被标记        |                                          |
| tag     | 创建一个引用源镜像标记目标镜像      |                                          |
| export  | 导出容器文件系统到tar归档文件       |                                          |
| import  | 导入容器文件tar归档文件创建镜像     |                                          |
| save    | 保存一个或多个镜像到一个tar归档文件 |                                          |
| load    | 加载镜像来自tar归档或标准输入       |                                          |

## 容器管理

### 创建容器常用选项

| 选项              | 描述                                                    |
| ----------------- | ------------------------------------------------------- |
| -i, –interactive  | 交互式                                                  |
| -t, –tty          | 分配一个伪终端                                          |
| -d, –detach       | 运行容器到后台                                          |
| -e, –env          | 设置环境变量                                            |
| -p, –publish list | 发布容器端口到主机                                      |
| -P, –publish-all  | 发布容器所有EXPOSE的端口到宿主机随机端口                |
| -name string      | 指定容器名称                                            |
| -h, –hostname     | 设置容器主机名                                          |
| -ip, string       | 指定容器ip，只能用于自定义网络                          |
| -network          | 连接容器到一个网络                                      |
| -mount, mount     | 将文件系统附加到容器                                    |
| -v, -volume list  | 绑定挂载一个卷                                          |
| -restart, string  | 容器退出时重启策略，默认no，可选值:[always\|on-failure] |

### 容器资源限制

| 选项                       | 描述                                       |
| -------------------------- | ------------------------------------------ |
| -m，–memory                | 容器可以使用的最大内存量                   |
| -memory-swap               | 允许交换到磁盘的内存量                     |
| -memory-swappiness=<0-100> | 容器使用SWAP分区交换的百分比(0-100，默认1) |
| -oom-kill-disable          | 禁用OOM Killer                             |
| -cpus                      | 可以使用的cpu数量                          |
| -cpuset-cpus               | 限制容器使用特定的cpu核心，如(0-3,0,1)     |
| -cpu-shares                | cpu共享(相对权重)                          |

**示例:
内存限额:**
允许容器最多使用500M内存和100M的Swap，并禁用 OOM Killer:

```shell
docker run -d --name nginx03 --memory="500m" --memory-swap=“600m" --oom-kill-disable nginx
```

**cpu限额**

允许容器最多使用一个半的CPU:

```shell
docker run -d --name nginx04 --cpus="1.5" nginx
```

允许容器最多使用50%的CPU：

```shell
docker run -d --name nginx05 --cpus=".5" nginx
```

### 管理容器常用命令

| 选项       | 描述                       | 示例 |
| ---------- | -------------------------- | ---- |
| ls         | 列出容器                   |      |
| inspect    | 查看一个或多个容器详细信息 |      |
| exec       | 在运行容器中执行命令       |      |
| commit     | 创建一个新镜像来自一个容器 |      |
| cp         | 拷贝文件/文件夹到一个容器  |      |
| logs       | 获取一个容器日志           |      |
| port       | 列出或指定容器端口映射     |      |
| top        | 显示一个容器运行的进程     |      |
| stats      | 显示容器资源使用情况       |      |
| stop/start | 停止/启动一个或多个容器    |      |
| rm         | 删除一个或多个容器         |      |

## 管理应用程序数据

### 将数据从宿主机挂载到容器中的三种方式

Docker提供三种方式将数据从宿主机挂载到容器中:

* volumes:Docker管理宿主机文件系统的一部分(/var/lib/docker/volumes)。保存数据的最佳方式。

* bind mounts:将宿主机上的任意位置的文件或者目录挂载到容器中。

* tmpfs:挂载存储在主机系统的内存中，而不会写入主机的文件系统。如果不希望将数据持久存储在任何位置，可以使用 tmpfs，同时避免写入容器可写层提高性能。 

## Dockerfile

### Dockerfile指令

| 指令        | 描述                                                   |
| ----------- | ------------------------------------------------------ |
| FROM        | 构建新镜像是基于哪个镜像                               |
| MAINTAINER  | 镜像维护者姓名或邮箱地址                               |
| RUN         | 构建镜像时运行的shell命令                              |
| COPY        | 拷贝文件或目录到镜像中                                 |
| ENV         | 设置环境变量                                           |
| USER        | 为RUN、CMD和ENTRYPOINT执行命令指定运行用户             |
| EXPOSE      | 声明容器运行的服务端口                                 |
| HEALTHCHECK | 容器中服务健康检查                                     |
| WORKDIR     | 为RUN、CMD、ENTRYPOINT、COPY和ADD设置工作目录          |
| ENTRYPOINT  | 运行容器时执行，如果有多个ENTRYPOINT指令，最后一个生效 |
| CMD         | 运行容器时执行，如果有多个CMD指令，最后一个生效        |

Dockerfile语法

```shell
Usage: docker build [OPTIONS] PATH | URL | - [flags] Options:
-t, --tag list # 镜像名称
-f, --file string # 指定Dockerfile文件位置
```

示例：

```shell
# docker build -t shykes/myapp .
# docker build -t shykes/myapp -f /path/Dockerfile /path
# docker build -t shykes/myapp http://www.exarmple.com/Dockerfile
```

### 构建业务基础镜像

#### 构建Nginx基础镜像

抒写dockerfile文件

```shell
[root@blsnt ~]# cd /server/tools
[root@blsnt tools]# cat Dockerfile-Nginx
FROM centos:7
ENV VERSION=1.15.5
MAINTAINER https://www.github/blsnt/blog
RUN yum install -y gcc gcc-c++ make \
    openssl-devel pcre-devel gd-devel \
    iproute net-tools telnet wget curl && \
    yum clean all && \
    rm -rf /var/cache/yum/*
RUN wget http://nginx.org/download/nginx-${VERSION}.tar.gz && \
    tar zxf nginx-${VERSION}.tar.gz && \
    cd nginx-${VERSION} && \
    ./configure --prefix=/usr/local/nginx \
    --with-http_ssl_module \
    --with-http_stub_status_module && \
    make -j 4 && make install && \
    rm -rf /usr/local/nginx/html/* && \
    echo "ok" >> /usr/local/nginx/html/status.html && \
    cd / && rm -rf nginx-${VERSION}* && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

ENV PATH $PATH:/usr/local/nginx/sbin
COPY nginx.conf /usr/local/nginx/conf/nginx.conf
WORKDIR /usr/local/nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

build构建

```shell
[root@blsnt tools]# docker build -t nginx:v1 -f Dockerfile-Nginx .
```

#### 构建PHP基础镜像

```shell
FROM centos:7
MAINTAINER https://www.github/blsnt/blog
RUN yum install epel-release -y && \
    yum install -y gcc gcc-c++ make gd-devel libxml2-devel \
    libcurl-devel libjpeg-devel libpng-devel openssl-devel \
    libmcrypt-devel libxslt-devel libtidy-devel autoconf \
    iproute net-tools telnet wget curl && \
    yum clean all && \
    rm -rf /var/cache/yum/*

RUN wget http://docs.php.net/distributions/php-5.6.36.tar.gz && \
    tar zxf php-5.6.36.tar.gz && \
    cd php-5.6.36 && \
    ./configure --prefix=/usr/local/php \
    --with-config-file-path=/usr/local/php/etc \
    --enable-fpm --enable-opcache \
    --with-mysql --with-mysqli --with-pdo-mysql \
    --with-openssl --with-zlib --with-curl --with-gd \
    --with-jpeg-dir --with-png-dir --with-freetype-dir \
    --enable-mbstring --with-mcrypt --enable-hash && \
    make -j 4 && make install && \
    cp php.ini-production /usr/local/php/etc/php.ini && \
    cp sapi/fpm/php-fpm.conf /usr/local/php/etc/php-fpm.conf && \
    sed -i "90a \daemonize = no" /usr/local/php/etc/php-fpm.conf && \
    mkdir /usr/local/php/log && \
    cd / && rm -rf php* && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

ENV PATH $PATH:/usr/local/php/sbin
COPY php.ini /usr/local/php/etc/
COPY php-fpm.conf /usr/local/php/etc/
WORKDIR /usr/local/php
EXPOSE 9000
CMD ["php-fpm"]
```

#### 构建Tomcat基础镜像

```shell
FROM centos:7
MAINTAINER https://www.github/blsnt/blog

ENV VERSION=8.5.43

RUN yum install java-1.8.0-openjdk wget curl unzip iproute net-tools -y && \
    yum clean all && \
    rm -rf /var/cache/yum/*

RUN wget http://mirror.bit.edu.cn/apache/tomcat/tomcat-8/v${VERSION}/bin/apache-tomcat-${VERSION}.tar.gz && \
    tar zxf apache-tomcat-${VERSION}.tar.gz && \
    mv apache-tomcat-${VERSION} /usr/local/tomcat && \
    rm -rf apache-tomcat-${VERSION}.tar.gz /usr/local/tomcat/webapps/* && \
    mkdir /usr/local/tomcat/webapps/test && \
    echo "ok" > /usr/local/tomcat/webapps/test/status.html && \
    sed -i '1a JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom"' /usr/local/tomcat/bin/catalina.sh && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

ENV PATH $PATH:/usr/local/tomcat/bin

WORKDIR /usr/local/tomcat

EXPOSE 8080
CMD ["catalina.sh", "run"]
```

#### 快速部署LNMP网站平台

![](http://images.dregs.top/images/20190726123525.png)

* 自定义网络

```shell
docker network create lnmp
```

* 创建Mysql容器

```shell
docker run -d \
--name lnmp_mysql \
--net lnmp \
--mount src=mysql-vol,dst=/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_DATABASE=wordpress mysql:5.7 --character-set-server=utf8
```

* 创建PHP容器

```shell
docker run -d \
--name lnmp_php \
--mount src=wwwroot,dst=/wwwroot php:v1
```

* 创建Nginx容器

```shell
docker run -d \
--name lnmp_nginx \
--net lnmp -p 99:80 \
--mount type=bind,src=$(pwd)/nginx.conf,dst=/usr/local/nginx/conf/nginx.conf \
--mount src=wwwroot,dst=/wwwroot nginx:v1
```

## 企业级镜像仓库Harbor

### harbor概述

Habor是由VMWare公司开源的容器镜像仓库。事实上，Habor是在Docker Registry上进行了相应的
企业级扩展，从而获得了更加广泛的应用，这些新的企业级特性包括:管理用户界面，基于角色的访
问控制 ，AD/LDAP集成以及审计日志等，足以满足基本企业需求。

[官方地址](https://vmware.github.io/harbor/cn/)

| 组件               | 功能                                      |
| ------------------ | ----------------------------------------- |
| harbor-adminserver | 配置管理中心                              |
| harbor-db          | Mysql数据库                               |
| harbor-jobservice  | 负责镜像复制                              |
| harbor-log         | 记录操作日志                              |
| harbor-ui          | Web管理页面和API                          |
| nginx              | 前端代理，负责前端页面和镜像上传/下载转发 |
| redis              | 会话                                      |
| registry           | 镜像存储                                  |

### Harbor部署

[harbor下载地址](https://github.com/goharbor/harbor/releases)

Harbor offline installer 离线安装

Harbor online installer 在线安装

**Harbor安装有3种方式:** 

* 在线安装:从Docker Hub下载Harbor相关镜像，因此安装软件包非常小
* 离线安装:安装包包含部署的相关镜像，因此安装包比较大

* OVA安装程序:当用户具有vCenter环境时， 使用此程序，在部署OVA后启动Harbor

安装docker-compose(如已安装请忽略)

```shell
curl -L https://github.com/docker/compose/releases/download/1.24.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

安装harbor

```shell
mkdir /application
cd /server/tools
tar xvf harbor-offline-installer-v1.6.1.tgz -C /application/
cd /application/harbor
vim harbor.cfg
hostname = 192.168.11.5
ui_url_protocol = http 
harbor_admin_password = 123456

#安装
./install.sh
```

> 浏览器访问：192.168.11.5 即可

### 基本使用

* 配置http镜像仓库可信任

```shell
vim /etc/docker/daemon.json
{"registry-mirrors": ["http://f1361db2.m.daocloud.io"],
"insecure-registries":["192.168.11.5"]}
#重启docker
systemctl restart docker
```

* docker登录harbor

```shell
docker login 192.168.11.5
Username: admin
Password: 密码123456
```

* 打标签

```shell
docker tag centos:7 192.168.11.5/obj/centos:v1
```

* 上传

```shell
docker push 192.168.11.5/obj/centos:v1
```

* 下载

```shell
docker pull 192.168.11.5/obj/centos:v1
```

## **Jenkins与Docker的自动化CI/CD流水线实战**

* 什么是CI/CD

持续集成(Continuous Integration，CI):代码合并、构建、部署、测试都在一起，不断地执行这个过程，并对结果反馈。
持续部署(Continuous Deployment，CD):部署到测试环境、预生产环境、生产环境。
持续交付(Continuous Delivery，CD):最终将产品发布到生产环境，给用户使用

* 高效的CI/CD环境可以获得:
  * 及时发现问题
  * 大幅度减少故障率
  * 加快迭代速度
  * 减少时间成本

* 发布流程设计

在如今的互联网时代，随着软件开发复杂度的不断提高，软件开发和发布管理也越来越重要。目前已经形成一套标准的流程，最重要的组成部分就是持续集成（Continuous Integration，CI）及持续部署、交付（CD）。在此，我们来以一个案例初步了解 CI 流程。那么什么是 CI 呢？简单来讲，CI 就是将传统的代码合并、构建、部署、测试都集成在一起，不断地执行这个过程，并对结果进行反馈。

CI 流程设计图：

![](http://images.dregs.top/images/20190726192234.png)

![](http://images.dregs.top/images/20190726192254.png)

* 工作流程：
  * 开发人员提交代码到Git版本仓库；	
  * Jenkins人工/定时触发项目构建；
  * Jenkins拉取代码、代码编码、打包镜像、推送到镜像仓库；
  * Jenkins在Docker主机创建容器并发布

* 主机环境规划

| 主机          | 描述           |
| ------------- | -------------- |
| 192.168.11.12 | harbor镜像仓库 |
| 192.168.11.11 | jenkins        |
| 192.168.11.10 | git仓库        |

* 部署Git仓库并上传测试代码

```shell
[root@git ~]# yum -y install git
[root@git ~]# useradd git
[root@git ~]# passwd git
[root@git ~]# su - git
[git@git ~]$ mkdir tomcat-java-demo.git
[git@git ~]$ cd tomcat-java-demo.git/
[git@git tomcat-java-demo.git]$ git --bare init
```

jenkins服务器拉取仓库并上传测试demo代码

```shell
[root@jenkins ~]# git clone git@192.168.11.10:/home/git/tomcat-java-demo.git
正克隆到 'tomcat-java-demo'...
The authenticity of host '192.168.11.10 (192.168.11.10)' can't be established.
ECDSA key fingerprint is SHA256:N2N7OCqVPMqpzZjyh1FDTf2V0sWZvOCNgNKOWl8sPFo.
ECDSA key fingerprint is MD5:d9:73:f9:e2:05:b9:b4:7e:da:5e:c8:ff:22:61:19:0e.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.11.10' (ECDSA) to the list of known hosts.
git@192.168.11.7's password:
warning: 您似乎克隆了一个空版本库。
[root@jenkins ~]# mv tomcat-java-demo/ tomcat-java-demo.bak
[root@jenkins ~]# git clone https://github.com/dingkai163/tomcat-java-demo.git
[root@jenkins tomcat-java-demo]# git config --global user.email "921390093@qq.com"
[root@jenkins tomcat-java-demo]# git config --global user.name "blsnt"
#修改仓库地址url
[root@jenkins tomcat-java-demo]# cat .git/config
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = git@192.168.11.10:/home/git/tomcat-java-demo.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
[root@jenkins tomcat-java-demo]# git add .
[root@jenkins tomcat-java-demo]# git commit -m "update"
[root@jenkins tomcat-java-demo]# git push origin master
```

* 部署Harbor镜像仓库

安装请参考上面有教程

构建Tomcat基础镜像，并推送到harbor镜像仓库

```shell
[root@git tools]# cd /server/tools
[root@git tools]# cat Dockerfile-tomcat
FROM centos:7
MAINTAINER https://www.github/blsnt/blog

ENV VERSION=8.5.43

RUN yum install java-1.8.0-openjdk wget curl unzip iproute net-tools -y && \
    yum clean all && \
    rm -rf /var/cache/yum/*

RUN wget http://mirror.bit.edu.cn/apache/tomcat/tomcat-8/v${VERSION}/bin/apache-tomcat-${VERSION}.tar.gz && \
    tar zxf apache-tomcat-${VERSION}.tar.gz && \
    mv apache-tomcat-${VERSION} /usr/local/tomcat && \
    rm -rf apache-tomcat-${VERSION}.tar.gz /usr/local/tomcat/webapps/* && \
    sed -i '1a JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom"' /usr/local/tomcat/bin/catalina.sh && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

ENV PATH $PATH:/usr/local/tomcat/bin

WORKDIR /usr/local/tomcat

EXPOSE 8080
CMD ["catalina.sh", "run"]
[root@git tools]# docker build -t tomcat:v1 -f Dockerfile-tomcat .
[root@git tools]# docker tag tomcat:v1 192.168.11.12/obj/tomcat:v1
[root@git tools]# docker login 192.168.11.12
[root@git tools]# docker push 192.168.11.12/obj/tomcat:v1
```

* 准备Jenkins环境并配置全局工具

部署jdk环境及maven

```shell
[root@jenkins ~]# mkdir /server/tools -p
[root@jenkins ~]# cd /server/tools
[root@jenkins tools]# tar xf jdk-8u45-linux-x64.tar.gz
[root@jenkins tools]# mv jdk1.8.0_45/ /usr/local/jdk
[root@jenkins tools]# vim /etc/profile
JAVA_HOME=/usr/local/jdk 
PATH=$PATH:$JAVA_HOME/bin  
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar 
export JAVA_HOME PATH CLASSPATH
[root@jenkins tools]# source /etc/profile
[root@jenkins tools]# java -version
java version "1.8.0_45"
Java(TM) SE Runtime Environment (build 1.8.0_45-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.45-b02, mixed mode)
[root@jenkins tools]# tar xf apache-maven-3.5.0-bin.tar.gz
[root@jenkins tools]# mv apache-maven-3.5.0 /usr/local/maven
```

安装jenkins，下载tomcat二进制包将war包放到webapps下即可

```shell
[root@jenkins tools]# wget http://mirrors.jenkins.io/war-stable/latest/jenkins.war
[root@jenkins tools]# wget http://mirror.bit.edu.cn/apache/tomcat/tomcat-8/v8.5.43/bin/apache-tomcat-8.5.43.tar.gz
[root@jenkins tools]# mv apache-tomcat-8.5.43 /usr/local/tomcat-jenkins
[root@jenkins tools]# rm -rf /usr/local/tomcat-jenkins/webapps/*
[root@jenkins tools]# mv jenkins.war /usr/local/tomcat-jenkins/webapps/ROOT.war
[root@jenkins tools]# cd /usr/local/tomcat-jenkins/bin/
[root@jenkins bin]# ./startup.sh
#获取密码
[root@jenkins bin]# tail -f /usr/local/tomcat-jenkins/logs/catalina.out
af3faf779b96470ebdefa82338d478cc
```

启动后，浏览器访问http://192.168.11.11:8080/，按提示输入密码，登录即可

![](http://images.dregs.top/images/20190726214652.png)

![](http://images.dregs.top/images/20190726215004.png)

![](http://images.dregs.top/images/20190726215058.png)

![](http://images.dregs.top/images/20190726215129.png)

![](http://images.dregs.top/images/20190726215149.png)

![](http://images.dregs.top/images/20190726215252.png)

* Jenkins安装必要插件

由于jenkins是离线安装，所有在此需要配置一下插件下载地址：系统管理-->插件管理-->Advanced

![](http://images.dregs.top/images/20190726233014.png)

![](http://images.dregs.top/images/20190726233101.png)

修改下方地址，将https修改为http 再点Submit

![](http://images.dregs.top/images/20190726233129.png)

Submit后点击Available，Check now此时我们可以看到很多可获得插件

![](http://images.dregs.top/images/20190726233154.png)

首先搜索并安装Pipeline插件
pipeline 是一套运行于jenkins上的工作流框架，将原本独立运行于单个或者多个节点的任务连接起来，实现单个任务难以完成的复杂流程编排与 可视化。

![](http://images.dregs.top/images/20190726233224.png)

再安装SCM to job 插件 git 插件，同上步骤（搜索，安装）。

* 项目创建

创建jobs

![](http://images.dregs.top/images/20190727153921.png)

选择流水线类型

![](http://images.dregs.top/images/20190727154356.png)

到这里我们就开始配置Pipeline script，点击Pipeline语法，来自动生成我们需要的配置。

![](http://images.dregs.top/images/20190727154422.png)

如下图，我们Git方式，配置Git仓库地址，再添加认证相关。

![](http://images.dregs.top/images/20190727154913.png)

这里我们使用的是秘钥认证方式，需要将jenkins上生成的公钥发送到git服务器上，然后将jenkins上的生成的私钥内容粘贴到下图Key中，这样jenkins就可以免交互的拉取git仓库中的代码了。

```shell
[root@jenkins ~]# ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:Ft/CWmT6wzqzOvR5qpf6/OsGoEkVdRtBs9pnftS9pt8 root@jenkins
The key's randomart image is:
+---[RSA 2048]----+
|      .o..*.     |
|      .  . =     |
|     .  . =      |
|    . .  X .   ..|
|   . o .S * + . o|
|    o ...= = .  .|
|     . ..++ . .o |
|      ..B.o. .o .|
|      +B*X+. ...E|
+----[SHA256]-----+

[root@jenkins ~]# ssh-copy-id git@192.168.11.11
```

jenkins添加私钥

![](http://images.dregs.top/images/20190727155452.png)

![](http://images.dregs.top/images/20190727155546.png)

![](http://images.dregs.top/images/20190727155643.png)

![](http://images.dregs.top/images/20190727155743.png)

![](http://images.dregs.top/images/20190727155409.png)

Pipeline生成代码

![](http://images.dregs.top/images/20190727155958.png)

配置完成后，我们就可以生成Pipeline脚本了。点击下方Generate Pipeline Script，然后复制方框内的内容

![](http://images.dregs.top/images/20190727160053.png)

编写我们所需要的Pipeline脚本如下，将其粘贴到script的拉取代码模块中，并修改分支master为${branch}，其他模块内容自行编写。

```shell
node { 
   // 拉取代码
   stage('Git Checkout') { 
       checkout([$class: 'GitSCM', branches: [[name: '$branch']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '3fc081d3-be7d-442a-b955-f6860086b88b', url: 'git@192.168.11.10:/home/git/tomcat-java-demo.git']]])
   }
   // 代码编译
   stage('Maven Build') {
        sh '''
        export JAVA_HOME=/usr/local/jdk
        /usr/local/maven/bin/mvn clean package -Dmaven.test.skip=true
        '''
   }
   // 项目打包到镜像并推送到镜像仓库
   stage('Build and Push Image') {
sh '''
REPOSITORY=192.168.11.12/obj/tomcat-java-demo:${branch}
cat > Dockerfile << EOF
FROM 192.168.11.12/obj/tomcat:v1 
MAINTAINER blsnt
RUN rm -rf /usr/local/tomcat/webapps/*
ADD target/*.war /usr/local/tomcat/webapps/ROOT.war
EOF
docker build -t $REPOSITORY .
docker login 192.168.11.12 -u admin -p 12345
docker push $REPOSITORY
'''
   }
   // 部署到Docker主机,远程主机的话可以使用ssh登录执行命令
   stage('Deploy to Docker') {
        sh '''
        REPOSITORY=192.168.11.12/obj/tomcat-java-demo:${branch}
        docker rm -f tomcat-java-demo |true
        docker pull $REPOSITORY
        docker container run -d --name tomcat-java-demo -p 88:8080 $REPOSITORY
        '''
   }
}
```



![](http://images.dregs.top/images/20190727233430.png)

在Pipeline脚本里面我们指定了一个branch参数，所以我们需要传递一个参数变量，这里我们选择参数化构建，默认值为master分支。

![](http://images.dregs.top/images/20190727233503.png)

然后保存配置

开始构建

![](http://images.dregs.top/images/20190727233528.png)

可以通过Console Output输出查看jenkins构建流程

![](http://images.dregs.top/images/20190727233640.png)

成功构建会提示： SUCCESS

![](http://images.dregs.top/images/20190727233734.png)

我们也可以查看构建成功后的图形构建过程

![](http://images.dregs.top/images/20190727233757.png)

通过浏览器来访问tomcat-java-demo项目：http://192.168.11.11:88/

![](http://images.dregs.top/images/20190727233853.png)

部署完成
