.. contents::
   :depth: 3
..

[TOC]

SSH批量分发脚本
===============

``gather_facts: no`` 来禁止 Ansible 收集 facts
信息，但是有时候又需要使用 facts 中的内容，这时候可以设置 facts 的缓存。

.. code:: python

   #!/bin/bash    
   # create key pair
       \rm /root/.ssh/id_rsa* -f
       ssh-keygen -t rsa -f /root/.ssh/id_rsa -P "" &>/dev/null
       
       # fenfa
       for ip in 7 31 41
       do
         echo =====================172.16.1.$ip fenfa info==========================
         sshpass -p123456 ssh-copy-id -i /root/.ssh/id_rsa.pub 172.16.1.$ip -o StrictHostKeyChecking=no
         sshpass -p '666666' ssh-copy-id -o StrictHostKeyChecking=no root@192.168.2.222
         echo =====================172.16.1.$ip fenfa end===========================
         echo ""
       done

ansible安装
===========

.. code:: python

   yum -y install ansible

ansible命令使用
===============

hosts配置主机清单

.. code:: python

   vim /etc/ansible/hosts
   [webserver]          # 分组
   192.168.11.11:222    # 非22端口加端口号

   [dbserver]
   192.168.11.12

   [appserver]
   192.168.11.1[1:2]  

输入密码执行ping测试

.. code:: python

   ansible 192.168.11.11 -m ping -k
   ansible all -m ping -k   # all所有主机
   ansible all -m command -a 'ls /root' -u wang -k -b -K    # 使用wang远程sudo root执行命令

取消key的检查并记录日志

.. code:: python

   vim /etc/ansible/ansible.cfg
   host_key_checking = False  # 取消注释
   log_path = /var/log/ansible.log  # 取消注释

查看被管理的所有主机

.. code:: python

   ansible all --list-hosts
   ansible-inventory -i /etc/ansible/prod_hosts  --graph

免密钥验证

.. code:: python

   ssh-keygen -t rsa # 一路回车

   ssh-copy-id 192.168.11.11

ansible的host-pattern

.. code:: python

   逻辑与:在webserver组并且在dbserver组中的主机
   ansible "webserver:&dbserver" -m ping

   逻辑非:在webserver组，但不在dbserver组中的主机
   ansible 'webserver:!dbserver' -m ping # 注意单引号

   综合逻辑:即在webserver又在dbserver并且在appserver但是不在ftpserver中的主机
   ansible 'webserver:dbserver:&appserver:!ftpserver' -m ping

ansible常用模块详解
===================

lineinfile文本修改模块
----------------------

详情请参考：https://www.cnblogs.com/jackchen001/p/6710233.html

**在匹配的内容前或后增加一行**

.. code:: python

   [root@master test]# cat http.conf 
   #Listen 12.34.56.78:80
   #Listen 80
   #Port

insertbefore匹配内容在前面添加

.. code:: python

       - name: httpd.conf modify 8080
         lineinfile:
            dest: /opt/playbook/test/http.conf
            regexp: '^Listen'
            insertbefore: '^#Port'   
            line: 'Listen 8080'
         tags:
          - http8080

验证

.. code:: python

   [root@master test]# cat http.conf 
   #Listen 12.34.56.78:80
   #Listen 80
   Listen 8080
   #Port

insertafter匹配内容在后面添加

.. code:: python

   - name: httpd.conf modify 8080
         lineinfile:
            dest: /opt/playbook/test/http.conf
            regexp: '^Listen'
            insertafter: '^#Port'   
            line: 'Listen 8080'
         tags:
          - http8080

验证

.. code:: python

   [root@master test]# cat http.conf 
   #Listen 12.34.56.78:80
   #Listen 80
   #Port
   Listen 8080

实例：

.. code:: python

   ---
   - hosts: 172.16.4.159
     remote_user: root
     vars:
       - app_name: ypsx-demo
     tasks:
       - name: updata filebeat.conf
         lineinfile:
           dest: /root/ypsx.yml
           insertafter: 'paths:'
           line: '    - /home/admin/logs/{{ 应用变量名 }}.log'

command模块
-----------

.. code:: python

   ansible all -m commmand -a 'service vsftpd start' # 默认模块，不支持$HOSTNAME < > |; &等

shell模块
---------

.. code:: python

   ansible all -m shell -a "hostname"  # 支持管道符号,相当于shell执行命令

script模块
----------

.. code:: python

   ansible all -m script -a '/root/ansible/host.sh'  # 在ansible创建脚本，本地执行到远程机器

copy模块
--------

.. code:: python

   ansible all -m copy -a 'src=/root/ansible/selinux dest=/etc/selinux/config backup=yes mode=644 ower=root' # 复制文件并备份源文件,修改权限，属主
   ansible all -m copy -a 'content="hello" dest=/etc/test.txt backup=yes mode=644 ower=root'
   # 自己写文件内容发送到服务器

fetch模块
---------

从客户端取文件至服务器端，copy相反,必须是单个文件

.. code:: python

   ansible all -m fetch -a 'src=/var/log/messages dest=/data'
   [root@master .ssh]# ll /data
   总用量 0
   drwxr-xr-x 3 root root 17 10月 18 06:51 192.168.11.11
   drwxr-xr-x 3 root root 17 10月 18 06:51 192.168.11.12

file模块
--------

.. code:: python

   ansible all -m file -a 'path=/tmp/f3 state=touch'  # 新建文件
   ansible all -m file -a 'path=/tmp/f3 state=absent'  # 删除
   ansible all -m file -a 'path=/data/dir1 state=directory' # 新建文件夹
   ansible all -m file -a 'path=/data/dir1 state=absent' # 删除
   ansible all -m file -a 'path=/etc/fstab dest=/tmp/fstab.link state=link' # 创建软连接
   ansible all -m file -a 'dest=/tmp/fstab.link state=absent' # 创建软连接

hostname模块
------------

.. code:: python

   ansible 192.168.11.11 -m hostname -a 'name=node1' # 修改主机名

cron模块
--------

.. code:: python

   ansible 192.168.11.11 -m cron -a 'minute=* hour=0 day=* month=* weekday=1,3,5 job="/usr/wall FBI warning"  name=warningcron'  # 设置定时任务
   ansible 192.168.11.11 -m cron -a 'disabled=true job="/usr/wall FBI warning"  name=warningcron'  # 注释定时任务

yum模块
-------

.. code:: python

   ansible all -m yum -a 'name=vsftpd'  # 安装，多个使用,分割
   ansible all -m yum -a 'name=vsftpd state=absent'  # 卸载

service模块
-----------

.. code:: python

   ansible all -m service -a 'name=vsftpd state=started enabled=yes' # 启动并且开机自启动
   ansible all -m service -a 'name=vsftpd state=restarted'
   ansible all -m service -a 'name=vsftpd state=stopped'

user模块
--------

.. code:: python

   ansible all -m user -a 'name=nginx shell=/sbin/nologin system=yes home=/home/nginx'
   ansible all -m user -a 'name=nginx state=absent remove=yes'

ansible-galaxy
--------------

.. code:: python

   ansible-galaxy install geerlingguy.nginx
   ansible-galaxy list geerlingguy.nginx
   ansible-galaxy remove geerlingguy.nginx

对文件进行加密
--------------

.. code:: python

   ansible-vault encrype hello.yml  # 输入口令
   ansible-vault decrypt hello.yml  # 输入口令
   ansible-vault rekey hello.yml    # 修改口令

ansible-console
---------------

.. code:: python

   root@webserver (1)[f:5]$
   [root@master ~]# ansible-console
   Welcome to the ansible console.
   Type help or ? to list commands.

   root@all (2)[f:5]$ cd webserver
   root@webserver (1)[f:5]$ forks 10
   root@webserver (1)[f:10]$ command hostname
   192.168.11.11 | CHANGED | rc=0 >>
   node1

YAML语法简介
============

ansible playbook基础
====================

简单事列

.. code:: python

   ---
   - hosts: webserver
     remote_user: root

     tasks:
       - name: hello
         command: hostname

playbook变量，tags，handlers使用
--------------------------------

handlers配合notify实现httpd配置发生变化自动重载配置
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: yaml

   ---
   - hosts: webserver
     remote_user: root

     tasks:
       - name: install package
         yum: name=httpd
       - name: copy conf file
         copy: src=files/httpd.conf dest=/etc/httpd/conf/ backup=yes
         notify: restart service   # 配置文件修改之后就重启
       - name: start service
         service: name=httpd state=started enabled=yes

     handlers:             
       - name: restart service
         service: name=httpd state=restarted  # 对应上面的notify

tags单独执行某个标签
~~~~~~~~~~~~~~~~~~~~

.. code:: yaml

   ---
   - hosts: webserver
     remote_user: root

     tasks:
       - name: install package
         yum: name=httpd
               tags: inhttpd
       - name: copy conf file
         copy: src=files/httpd.conf dest=/etc/httpd/conf/ backup=yes
         notify: restart service   
       - name: start service
         service: name=httpd state=started enabled=yes
         tags: rshttpd

     handlers:             
       - name: restart service
         service: name=httpd state=restarted 

.. code:: yaml

   ansible-playbook -t rshttpd httpd.yml # 单独执行某个标签
   ansible-playbook -t rshttpd,inhttpd httpd.yml # 执行多个标签
   ansible-playbook -t rshttpd httpd.yml # 多个tags可以共用一个标签
   ansible-playbook httpd.yml --list-tags # 查看标签信息

playbook变量指定单个变量
~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

   ---
   - hosts: webserver
     remote_user: root

     tasks:
       - name: install package
         yum: name={{ pkname }}
       - name: start service
         service: name={{ pkname }} state=started enabled=yes

.. code:: python

   ansible-playbook -e 'pkname=vsftpd' package.yml # 指定变量

playbook变量指定多个变量
~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

   ---
   - hosts: webserver
     remote_user: root

     tasks:
       - name: install package
         yum: name={{ pkname1 }}
       - name: install package
         yum: name={{ pkname2 }}

.. code:: python

   ansible-playbook -e 'pkname1=vsftpd pkname2=redis' package.yml # 指定变量

声明变量
~~~~~~~~

.. code:: python

   ---
   - hosts: webserver
     remote_user: root
     vars:
        - pkname1: httpd
        - pkname2: redis

     tasks:
       - name: install package
         yum: name={{ pkname1 }}
       - name: install package
         yum: name={{ pkname2 }}

.. code:: python

   ansible-playbook package.yml

在/etc/ansible/hosts 中定义单个变量
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

   [dbserver]
   192.168.11.12 http_port=81
   192.168.11.11 http_port=82

.. code:: python

   ---
   - hosts: dbserver
     remote_user: root
       
     tasks:
       - name: set hostname
         hostname: name=blsnt{{ http_port }}

.. code:: python

   ansible-playbook host.yml
   ansible dbserver -a 'hostname'  # 已经修改
   192.168.11.12 | CHANGED | rc=0 >>
   blsnt81

   192.168.11.11 | CHANGED | rc=0 >>
   blsnt82

在/etc/ansible/hosts 中定义组变量
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

   [dbserver:vars]
   nodename=www
   domainname=blsnt

   [dbserver]
   192.168.11.12 http_port=81
   192.168.11.11 http_port=82

.. code:: python

   ---
   - hosts: dbserver
     remote_user: root

     tasks:
       - name: set hostname
         hostname: name={{ nodename }}{{ http_port }}.{{ domainname }}

.. code:: python

   ansible-playbook host.yml
   ansible dbserver -a 'hostname'
   192.168.11.12 | CHANGED | rc=0 >>
   www81.blsnt

   192.168.11.11 | CHANGED | rc=0 >>
   www82.blsnt

通过命令行执行变量优先级最高
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

   ansible-playbook -e varname=value

在playbook中定义
~~~~~~~~~~~~~~~~

.. code:: yml

   vars:
     - var1: value1
     - var2: value2

在独立变量yaml文件中定义
~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: yml

   cat vars.yml
   var1: httpd
   var2: nginx

调用

.. code:: yml

   cat var.yml
   - hosts: web
     remote_user: root
     vars_files:
       - vars.yml
     tasks:
       - name: create httpd log
         file: name=/app/{{ var1 }}.log state=touch
       - name: create nginx log
         file: name=/app/{{ var2 }}.log state=touch

playbook模板templates
---------------------

通过ansible内置的变量自动设置nginx的worker数量

.. code:: yml

   ansible webserver -m setup|grep "cpu"
   ansible webserver -m setup -a 'filter=ansible_os_family'

将nginx模板配置文件worker数量修改为变量，自动根据机器核数生成

.. code:: yml

   worker_processes {{ ansible_processor_vcpus }}

.. code:: yml

   - hosts: webserver
     remote_user: root
     
     tasks:
       - name: install package
         yum: name=nginx
       - name: copy templeate
         template: src=nginx.conf.j2 dest=/etc/nginx/nginx.conf
       - name: start service
         service: name=nginx state=started enabled=yes

playbooke条件判断when
---------------------

.. code:: yml

   - hosts: webserver
     remote_user: root
     
     tasks:
       - name: install package
         yum: name=nginx
       - name: copy templeate for centos7
         template: src=nginx.conf7.j2 dest=/etc/nginx/nginx.conf
         when: ansible_distribution_major_version == "7"
       - name: copy templeate for centos6
         template: nginx.conf6.j2 dest=/etc/nginx/nginx.conf
         when: ansible_distribution_major_version == "6"
       - name: start service
         service: name=nginx state=started enabled=yes

playbook字典with_items
======================

.. code:: yml

   - hosts: webserver
     remote_user: root
     
     tasks:
       - name: create some file
         file: name=/data/{{ item }} state=touch
         when: ansible_distribution_major_version == "7"
         with_items:
           - file1
           - file2
           - file3
       - name: install some packages
         yum: name={{ item }}
         with_items:
           - httpd
           - vsftpd
           - sl

.. code:: yml

   - hosts: webserver
     remote_user: root
     
     tasks:
       - name: create some groups
         group: name={{ item }}
         when: ansible_distribution_major_version == "7"
         with_items:
           - g1
           - g2
           - g3
       - name: create some users
         user: name={{ item.name }} group={{ item.group }}
         with_items:
           - { name: 'user1', group: 'g1' }
           - { name: 'user2', group: 'g2' }
           - { name: 'user3', group: 'g3' }

playbook中template for if
=========================

.. code:: yml

   - hosts: webserver
     remote_user: root
     vars:
       ports:
         - 81
         - 82
         - 83
     tasks:
       - name: copy conf
         template: src=for1.conf.j2 dest=/data/for1.conf

.. code:: yml

   vim for1.conf.j2
   {% for port in ports %}
   server{
       listen {{ port }}
   }
   {% end %}

高级用法

.. code:: yml

   - hosts: webserver
     remote_user: root
     vars:
       ports:
         - web1:
                 port: 81
                 name: web1.blsnt.com
                 rootdir: /data/werb1
               - web2:
                 port: 82
                 name: web2.blsnt.com
                 rootdir: /data/web2
               - web3:
                 port: 83
                 name: web3.blsnt.com
                 rootdir: /data/web3
     tasks:
       - name: copy conf
         template: src=for1.conf.j2 dest=/data/for1.conf

.. code:: yml

   {% for p in ports %}
   server{
           listen {{ p.port }}
           servername {{ p.name }}
           documentroot{{ p.rootdir }}
   }
   {% end %}

If 判断

.. code:: yml

   - hosts: webserver
     remote_user: root
     vars:
       ports:
         - web1:
                 port: 81
                 #name: web1.blsnt.com
                 rootdir: /data/werb1
               - web2:
                 port: 82
                 name: web2.blsnt.com
                 rootdir: /data/web2
               - web3:
                 port: 83
                 name: web3.blsnt.com
                 rootdir: /data/web3
     tasks:
       - name: copy conf
         template: src=for1.conf.j2 dest=/data/for1.conf

.. code:: yml

   {% for p in ports %}
   server{
           listen {{ p.port }}
   {% if p.name is defined $%}
           servername {{ p.name }}
   {% endif %}
           documentroot{{ p.rootdir }}
   }
   {% end %}

ansible Roles
=============

目录规范

.. code:: python

   # 推荐路径/etc/ansible
   roles/
     nginx/
           file
       templates
       tasks
       handlers
       vars

.. code:: yml

   cat /etc/ansible/roles/nginx/tasks/group.yml
   - name: create group
     group: name=nginx git=80
     
   cat /etc/ansible/roles/nginx/tasks/user.yml
   - name: create user
     user: name=nginx uid=80 group=nginx system=yes shell=/sbin/nologin

   cat /etc/ansible/roles/nginx/tasks/yum.yml
   - name: install package
     yum: name=nginx
     
   cat /etc/ansible/roles/nginx/tasks/start.yml
   - name: start service
     service: name=nginx state=started enabled=yes

   cat /etc/ansible/roles/nginx/tasks/restart.yml
   - name: restart service
     service: name=nginx state=restarted
     
   cat /etc/ansible/roles/nginx/tasks/templ.yml
   - name: copy conf
     template: src=nginx.conf.j2 dest=/etc/nginx/nginx.conf

   cat /etc/ansible/roles/nginx/tasks/main.yml
   - include: group.yml
   - include: user.yml
   - include: yum.yml
   - include: templ.yml
   - include: start.yml

   cat /etc/ansible/nginx_roles.yml
   - hosts: webserver
     remote_user: root
     roles:
       - role: nginx

   # 需要定义好模板文件，我这里就没有定义了
   ansible-playbook nginx_roles.yml

同时调用角色

.. code:: yml

   - hosts: webserver
    remote_user: root
    roles:
      - role: httpd
      - rele: redis

角色之间任务调用

.. code:: yml

    cat /etc/ansible/roles/nginx/tasks/main.yml
   - include: group.yml
   - include: user.yml
   - include: yum.yml
   - include: templ.yml
   - include: start.yml
   - include: roles/httpd/tasks/copyfile.yml

标签选择

.. code:: yml

   cat /etc/ansible/some.role.yml
   - hosts: webserver
     remote_user: root
     roles:
       - { role: httpd, tags: ['web':'httpd'] }
       - { role: nginx, tags: ['web':'nginx'] }

   ansible-playbook -t web some_role.yml

标签选择加判断

.. code:: yml

   - hosts: all
     remote_user: root
     roles:
       - { role: httpd, tags: ['web':'httpd'] }
       - { role: nginx, tags: ['web':'nginx'],when: ansible_distribution_major_version == "7" }

.. code:: python

   include:
     - project: "arch-foundation/cicd"
       ref: test
       file: "/templates/Maven.gitlab-ci.yml"
