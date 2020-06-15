.. contents::
   :depth: 3
..

安装VMware虚拟化软件
====================

`软件百度云分享链接 <https://pan.baidu.com/s/1RzUAbcBfEtNV3n-VLIbx0A>`__
提取码为:1y2s 找到vmware双击安装，一直下一步就完事了，里面有激活码
|image0| 找到文件新建虚拟机 |image1| 默认自定义就行 |image2| 下一步
|image3| 选择稍后安装操作系统 |image4| 选择Linux 版本：centos64位
|image5| 定义虚拟机的名称和安装位置。可以默认 |image6| 选择CPU核数
|image7| 根据你的系统配置来调节内存。我这里默认1G |image8|
选择网络NAT地址转换模式 |image9| 默认即可 |image10| 默认即可 |image11|
默认即可 |image12| 选择磁盘空间大小 |image13| 默认即可 |image14| 完成
|image15|

安装操作系统CentOS7.6
=====================

`CentOS7.6下载地址 <https://mirrors.aliyun.com/centos/7.6.1810/isos/x86_64/>`__
`历史版本下载地址 <http://vault.centos.org/>`__

安装系统方法
------------

1. u盘安装
2. 网络安装，无人值守安装
3. 光盘安装(淘汰)，虚拟机使用光盘安装

安装操作系统
------------

点击编辑虚拟机设置 |image16| 选择镜像 |image17| 开启虚拟机 |image18|
鼠标点击进去按Tab键输入 biosdevname=0 net.ifnames=0 直接回车
更改网卡为eth0 |image19| 选择安装语言 |image20| 选择时间 |image21|
选择上海 |image22| 键盘默认即可
选择系统需要安装的语言。我这里只选择英语，你也可以添加中文 |image23|
|image24| 安装源默认即可 选择软件安装 |image25| |image26| 配置网络
|image27| |image28| 选择开机自动连接 |image29| |image30| |image31|
关闭安全(工作中建议打开) |image32| |image33| 磁盘格式化并分区 |image34|
自定义分区 |image35| 选择标准分区 |image36| |image37|
swap在工作中内存大于8g就给8g，小于8g就给内存的1.5倍 |image38| 根分配所有
|image39| |image40| |image41| |image42|
设置root密码(工作中必须8位以上，数字+字母+特殊字符) |image43| |image44|
等待安装完成 |image45| 最后点击reboot会重启。进入操作系统 |image46|
输入用户名：root 密码：666666 进入系统 虚拟机网络配置 |image47|
|image48| 可以使用ping命令检测是否可以上网 |image49|
如果不可以上网可以使用systemctl restart network重启网络
安装系统后：nmtui 命令可以使用图形化配置网卡和主机名 |image50|

安装xshell远程连接服务器
========================

`软件百度云分享链接 <https://pan.baidu.com/s/1RzUAbcBfEtNV3n-VLIbx0A>`__
提取码为:1y2s 找到xshell直接下一步下一步就完事了 安装完成新建一个会话
|image51| 连接配置 |image52| 输入用户名和密码 |image53| 点击连接
|image54| 接受并保存 |image55| 已经连接上服务器 |image56| xshell属性配置
文件->属性->终端 |image57| 基础命令 |image58| 安装下载工具wget |image59|
更改yum源 `阿里云源下载地址 <%5Bhttps://opsx.alibaba.com/mirror>`__
|image60| |image61| epel源同上

.. code:: bash

   $ wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
   $ wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

|image62| 将系统软件更新到最新

.. code:: bash

   $ yum update -y

CentOS6和CentOS7都要安装的运维常用基础工具包

.. code:: bash

   $ yum -y install tree nmap dos2unix lrzsz nc lsof wget tcpdump htop iftop iotop sysstat nethogs 

CentOS7要安装的运维常用基础工具包

.. code:: bash

   $ yum -y install psmisc net-tools bash-completion vim-enhanced -y

安装的时候选包，选了三个，组包：里面有一堆软件。如果没有安装请用以下方式安装

.. code:: bash

   $ yum groups mark convert
   $ yum groupslist 查看所有包组名称

|image63| 安装包组名称

.. code:: bash

   $ yum groupinstall "Development Tools" -y

.. |image0| image:: http://images.dregs.top/images/20190408191008.png
.. |image1| image:: http://images.dregs.top/images/20190408191125.png
.. |image2| image:: http://images.dregs.top/images/20190408191150.png
.. |image3| image:: http://images.dregs.top/images/20190408191225.png
.. |image4| image:: http://images.dregs.top/images/20190408191249.png
.. |image5| image:: http://images.dregs.top/images/20190408191327.png
.. |image6| image:: http://images.dregs.top/images/20190408191514.png
.. |image7| image:: http://images.dregs.top/images/20190408191555.png
.. |image8| image:: http://images.dregs.top/images/20190409113157.png
.. |image9| image:: http://images.dregs.top/images/20190408191648.png
.. |image10| image:: http://images.dregs.top/images/20190408191705.png
.. |image11| image:: http://images.dregs.top/images/20190409112034.png
.. |image12| image:: http://images.dregs.top/images/20190409112957.png
.. |image13| image:: http://images.dregs.top/images/20190409113054.png
.. |image14| image:: http://images.dregs.top/images/20190409113113.png
.. |image15| image:: http://images.dregs.top/images/20190409113132.png
.. |image16| image:: http://images.dregs.top/images/20190408193503.png
.. |image17| image:: http://images.dregs.top/images/20190409113452.png
.. |image18| image:: http://images.dregs.top/images/20190409113541.png
.. |image19| image:: http://images.dregs.top/images/20190408194118.png
.. |image20| image:: http://images.dregs.top/images/20190408194411.png
.. |image21| image:: http://images.dregs.top/images/20190409113710.png
.. |image22| image:: http://images.dregs.top/images/20190409113725.png
.. |image23| image:: http://images.dregs.top/images/20190408194854.png
.. |image24| image:: http://images.dregs.top/images/20190408194921.png
.. |image25| image:: http://images.dregs.top/images/20190408195011.png
.. |image26| image:: http://images.dregs.top/images/20190408195053.png
.. |image27| image:: http://images.dregs.top/images/20190409113750.png
.. |image28| image:: http://images.dregs.top/images/20190408195303.png
.. |image29| image:: http://images.dregs.top/images/20190409113807.png
.. |image30| image:: http://images.dregs.top/images/20190409113836.png
.. |image31| image:: http://images.dregs.top/images/20190409113851.png
.. |image32| image:: http://images.dregs.top/images/20190408195740.png
.. |image33| image:: http://images.dregs.top/images/20190408195812.png
.. |image34| image:: http://images.dregs.top/images/20190408195958.png
.. |image35| image:: http://images.dregs.top/images/20190408200048.png
.. |image36| image:: http://images.dregs.top/images/20190409113908.png
.. |image37| image:: http://images.dregs.top/images/20190408200451.png
.. |image38| image:: http://images.dregs.top/images/20190408200711.png
.. |image39| image:: http://images.dregs.top/images/20190408200733.png
.. |image40| image:: http://images.dregs.top/images/20190408200805.png
.. |image41| image:: http://images.dregs.top/images/20190408200844.png
.. |image42| image:: http://images.dregs.top/images/20190408200902.png
.. |image43| image:: http://images.dregs.top/images/20190409113930.png
.. |image44| image:: http://images.dregs.top/images/20190408200940.png
.. |image45| image:: http://images.dregs.top/images/20190408201030.png
.. |image46| image:: http://images.dregs.top/images/20190409185418.png
.. |image47| image:: http://images.dregs.top/images/20190409193435.png
.. |image48| image:: http://images.dregs.top/images/20190409193456.png
.. |image49| image:: http://images.dregs.top/images/20190409193538.png
.. |image50| image:: http://images.dregs.top/images/20190409193840.png
.. |image51| image:: http://images.dregs.top/images/20190409194254.png
.. |image52| image:: http://images.dregs.top/images/20190409194351.png
.. |image53| image:: http://images.dregs.top/images/20190409194420.png
.. |image54| image:: http://images.dregs.top/images/20190409194443.png
.. |image55| image:: http://images.dregs.top/images/20190409194501.png
.. |image56| image:: http://images.dregs.top/images/20190409194520.png
.. |image57| image:: http://images.dregs.top/images/20190409194654.png
.. |image58| image:: http://images.dregs.top/images/20190409195225.png
.. |image59| image:: http://images.dregs.top/images/20190409195632.png
.. |image60| image:: http://images.dregs.top/images/20190409200251.png
.. |image61| image:: http://images.dregs.top/images/20190409200308.png
.. |image62| image:: http://images.dregs.top/images/20190409200008.png
.. |image63| image:: http://images.dregs.top/images/20190409201403.png
