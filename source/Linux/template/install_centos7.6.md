## 安装VMware虚拟化软件

[软件百度云分享链接](https://pan.baidu.com/s/1RzUAbcBfEtNV3n-VLIbx0A) 提取码为:1y2s
找到vmware双击安装，一直下一步就完事了，里面有激活码
![](http://images.dregs.top/images/20190408191008.png)
找到文件新建虚拟机
![](http://images.dregs.top/images/20190408191125.png)
默认自定义就行
![](http://images.dregs.top/images/20190408191150.png)
下一步
![](http://images.dregs.top/images/20190408191225.png)
选择稍后安装操作系统
![](http://images.dregs.top/images/20190408191249.png)
选择Linux 版本：centos64位
![](http://images.dregs.top/images/20190408191327.png)
定义虚拟机的名称和安装位置。可以默认
![](http://images.dregs.top/images/20190408191514.png)
选择CPU核数
![](http://images.dregs.top/images/20190408191555.png)
根据你的系统配置来调节内存。我这里默认1G
![](http://images.dregs.top/images/20190409113157.png)
选择网络NAT地址转换模式
![](http://images.dregs.top/images/20190408191648.png)
默认即可
![](http://images.dregs.top/images/20190408191705.png)
默认即可
![](http://images.dregs.top/images/20190409112034.png)
默认即可
![](http://images.dregs.top/images/20190409112957.png)
选择磁盘空间大小
![](http://images.dregs.top/images/20190409113054.png)
默认即可
![](http://images.dregs.top/images/20190409113113.png)
完成
![](http://images.dregs.top/images/20190409113132.png)

## 安装操作系统CentOS7.6
[CentOS7.6下载地址](https://mirrors.aliyun.com/centos/7.6.1810/isos/x86_64/)
[历史版本下载地址](http://vault.centos.org/)

### 安装系统方法
1. u盘安装
2. 网络安装，无人值守安装
3. 光盘安装(淘汰)，虚拟机使用光盘安装

### 安装操作系统
点击编辑虚拟机设置
![](http://images.dregs.top/images/20190408193503.png)
选择镜像
![](http://images.dregs.top/images/20190409113452.png)
开启虚拟机
![](http://images.dregs.top/images/20190409113541.png)
鼠标点击进去按Tab键输入 biosdevname=0 net.ifnames=0 直接回车 更改网卡为eth0
![](http://images.dregs.top/images/20190408194118.png)
选择安装语言
![](http://images.dregs.top/images/20190408194411.png)
选择时间
![](http://images.dregs.top/images/20190409113710.png)
选择上海
![](http://images.dregs.top/images/20190409113725.png)
键盘默认即可
选择系统需要安装的语言。我这里只选择英语，你也可以添加中文
![](http://images.dregs.top/images/20190408194854.png)
![](http://images.dregs.top/images/20190408194921.png)
安装源默认即可
选择软件安装
![](http://images.dregs.top/images/20190408195011.png)
![](http://images.dregs.top/images/20190408195053.png)
配置网络
![](http://images.dregs.top/images/20190409113750.png)
![](http://images.dregs.top/images/20190408195303.png)
选择开机自动连接
![](http://images.dregs.top/images/20190409113807.png)
![](http://images.dregs.top/images/20190409113836.png)
![](http://images.dregs.top/images/20190409113851.png)
关闭安全(工作中建议打开)
![](http://images.dregs.top/images/20190408195740.png)
![](http://images.dregs.top/images/20190408195812.png)
磁盘格式化并分区
![](http://images.dregs.top/images/20190408195958.png)
自定义分区
![](http://images.dregs.top/images/20190408200048.png)
选择标准分区
![](http://images.dregs.top/images/20190409113908.png)
![](http://images.dregs.top/images/20190408200451.png)
swap在工作中内存大于8g就给8g，小于8g就给内存的1.5倍
![](http://images.dregs.top/images/20190408200711.png)
根分配所有
![](http://images.dregs.top/images/20190408200733.png)
![](http://images.dregs.top/images/20190408200805.png)
![](http://images.dregs.top/images/20190408200844.png)
![](http://images.dregs.top/images/20190408200902.png)
设置root密码(工作中必须8位以上，数字+字母+特殊字符)
![](http://images.dregs.top/images/20190409113930.png)
![](http://images.dregs.top/images/20190408200940.png)
等待安装完成
![](http://images.dregs.top/images/20190408201030.png)
最后点击reboot会重启。进入操作系统
![](http://images.dregs.top/images/20190409185418.png)
输入用户名：root 密码：666666 进入系统
虚拟机网络配置
![](http://images.dregs.top/images/20190409193435.png)
![](http://images.dregs.top/images/20190409193456.png)
可以使用ping命令检测是否可以上网
![](http://images.dregs.top/images/20190409193538.png)
如果不可以上网可以使用systemctl restart network重启网络
安装系统后：nmtui 命令可以使用图形化配置网卡和主机名
![](http://images.dregs.top/images/20190409193840.png)

## 安装xshell远程连接服务器
[软件百度云分享链接](https://pan.baidu.com/s/1RzUAbcBfEtNV3n-VLIbx0A) 提取码为:1y2s
找到xshell直接下一步下一步就完事了
安装完成新建一个会话
![](http://images.dregs.top/images/20190409194254.png)
连接配置
![](http://images.dregs.top/images/20190409194351.png)
输入用户名和密码
![](http://images.dregs.top/images/20190409194420.png)
点击连接
![](http://images.dregs.top/images/20190409194443.png)
接受并保存
![](http://images.dregs.top/images/20190409194501.png)
已经连接上服务器
![](http://images.dregs.top/images/20190409194520.png)
xshell属性配置 文件->属性->终端
![](http://images.dregs.top/images/20190409194654.png)
基础命令
![](http://images.dregs.top/images/20190409195225.png)
安装下载工具wget
![](http://images.dregs.top/images/20190409195632.png)
更改yum源
[阿里云源下载地址]([https://opsx.alibaba.com/mirror)
![](http://images.dregs.top/images/20190409200251.png)
![](http://images.dregs.top/images/20190409200308.png)
epel源同上
```bash
$ wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
$ wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```
![](http://images.dregs.top/images/20190409200008.png)
将系统软件更新到最新
```bash
$ yum update -y
```
CentOS6和CentOS7都要安装的运维常用基础工具包
```bash
$ yum -y install tree nmap dos2unix lrzsz nc lsof wget tcpdump htop iftop iotop sysstat nethogs 
```
CentOS7要安装的运维常用基础工具包
```bash
$ yum -y install psmisc net-tools bash-completion vim-enhanced -y
```
安装的时候选包，选了三个，组包：里面有一堆软件。如果没有安装请用以下方式安装
```bash
$ yum groups mark convert
$ yum groupslist 查看所有包组名称
```
![](http://images.dregs.top/images/20190409201403.png)
安装包组名称
```bash
$ yum groupinstall "Development Tools" -y
```
