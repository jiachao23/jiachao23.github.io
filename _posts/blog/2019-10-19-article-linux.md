---
title: Linux下常用软件安装
categories: [linux，software]
keywords: linux, 软件, 安装
description: Linux下常用软件安装
---


#  Linux下常用软件安装
> ### PS:待开发中。。。。
> ### 作者：jiac

## 安装目录

> * [软硬件环境](#软硬件环境)
> * [JDK](#jdk)
> * [Tomcat](#tomcat)
> * [Mysql](#mysql)
> * [Redis](#redis)
> * [Nginx](#nginx)
> * [RabbitMq](#RabbitMq)
> * [git](#git)
> * [gitlab](#gitlab)
> * [node](#node)
> * [yapi](#yapi)
> * [mongodb](#mongodb)
> * [docker](#docker)
> * [Jenkins](#jenkins)
> * [安装高版本gcc](#安装高版本gcc)
> * [LDAP](#LDAP)
> * [Confluence](#Confluence)
> * [docker](#docker)
> * [docker-compose](#docker-compose)
> * [Harbor](#Harbor)
> * [常用软件安装脚本](#useage)


## 软硬件环境

| 软件     | 相关软件包或版本                         |
| -------- | ---------------------------------------- |
| 操作系统 | CentOS Linux release 7.6.1810            |
| jdk      | jdk-8u221-linux-x64.tar.gz               |
| tomcat   | apache-tomcat-7.0.94.tar.gz              |
| mysql    | mysql-8.0.16-2.el7.x86_64.rpm-bundle.tar |
| redis    | redis-5.0.5.tar.gz                       |
| nginx    | nginx-1.6.2.build.tar.gz                 |
| git      | release                                  |
| gitlab   | release                                  |
| node     | node-v10.16.0.tar.gz                     |
| yapi     | release                                  |
| mongodb  | release                                  |
| docker   | release                                  |
| jenkins  | release                                  |
| gcc      | gcc-8.3.0.tar.gz                         |
| docker      |                          |
| docker-compose      | 1.24.1                         |
| Harbor      | 1.6.0                         |

## JDK


### 1、下载地址

 http://software.jcohy.com/linux/jdk-8u221-linux-x64.tar.gz 

### 2、常规安装

1、用FileZilla或其他工具将下载好的安装包上传至Linux服务器。放置在 **/opt/software/** 目录下。这里使用的是 **jdk-8u221-linux-x64.tar.gz**

2、解压jdk到/usr/local目录

```shell
tar -zxvf jdk-8u221-linux-x64.tar.gz -C /usr/local/
```

3、设置环境变量，在/etc/profile文件最后追加相关内容

```shell
vi /etc/profile

export JAVA_HOME=/usr/local/jdk1.8.0_221
export PATH=$PATH:$JAVA_HOME/bin
```

4、刷新环境变量

```shell
source /etc/profile
```

5、测试java命令是否可用

```shell
java -version
```

## Tomcat


### 1、下载地址

 http://software.jcohy.com/linux/apache-tomcat-7.0.94.tar.gz

### 2、常规安装

1、用FileZilla或其他工具将下载好的安装包上传至Linux服务器。放置在 **/opt/software/** 目录下。这里使用的是 **apache-tomcat-7.0.94.tar.gz**

2、解压tomcat

```shell
tar -zxvf apache-tomcat-7.0.94.tar.gz -C /usr/local/
```

3、启动tomcat

```shell
/usr/local/apache-tomcat-7.0.94/bin/startup.sh
```

4、查看tomcat进程是否启动

```shell
jps
```

5、查看tomcat进程端口

```shell
netstat -anpt | grep 2465
```

6、通过浏览器访问tomcat

 <http://192.168.11.231:8080/>

## mysql


### 1、下载地址

https://pan.baidu.com/s/1iP5QDXSYHQ5q61cBFQvs-A

### 2、常规安装

1. 用FileZilla或其他工具将下载好的安装包上传至Linux服务器。放置在 **/opt/software/** 目录下。这里使用的是 **mysql-8.0.16-2.el7.x86_64.rpm-bundle.tar**

2. 解压mysql

```shell
	mkdir -p /usr/local/mysql
	tar -xvf mysql-8.0.16-2.el7.x86_64.rpm-bundle.tar
```


3.  centos7默认集成了MariaDB，而安装mysql的话会和mariadb的文件冲突，所以需要先卸载掉mariadb

```shell
	yum remove mariadb
	rpm -qa|grep mariadb-libs
	rpm -e mariadb-libs-5.5.60-1.el7_5.x86_64 --nodeps
```

4. 安装相关依赖

```shell
yum search libaio  # 检索相关信息
yum install -y libaio # 安装依赖包

yum search net-tools
yum install -y net-tools

yum search perl
yum install -y perl 

yum install -y numactl
```

5. mysql rpm 安装顺序

```xml
rpm -ivh mysql-community-common-8.0.16-2.el7.x86_64.rpm   
rpm -ivh mysql-community-libs-8.0.16-2.el7.x86_64.rpm   
rpm -ivh mysql-community-client-8.0.16-2.el7.x86_64.rpm  
rpm -ivh mysql-community-server-8.0.16-2.el7.x86_64.rpm 
```

6.设置开机启动

```shell
systemctl enable mysqld
```

7.启动mysql

```shell
systemctl start mysqld
```

8、mysql安装完成之后，在/var/log/mysqld.log文件中给root生成了一个默认密码。通过下面的方式找到root默认密码，然后登录mysql进行修改：

```shell
grep 'temporary password' /var/log/mysqld.log

mysql -uroot -p
```

9、登录上去后，使用下列语句更新密码

```shell
 ALTER USER 'root'@'localhost' IDENTIFIED BY 'jia_chao23@126.com';
```

 mysql 5.8 修改密码加密方式，改成mysql_native_password,然后修改密码
```shell
 #查看密码加密方式
 use mysql
 select host,user,plugin  from mysql.user;
 

 ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'jia_chao23@126.com';
 ALTER USER 'root'@'%' IDENTIFIED BY 'jia_chao23@126.com';
 ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'jia_chao23@126.com';
 ALTER USER 'root'@'localhost' IDENTIFIED BY 'jia_chao23@126.com';
 flush privileges;

```

10、修改密码策略

mysql5.8默认安装了密码安全检查插件（validate_password），默认密码检查策略要求密码必须包含：大小写字母、数字和特殊符号，并且长度不能少于8位。否则会提示ERROR 1819 (HY000): Your
password does not satisfy the current policy requirements错误，如下图所示：

![](https://github.com/jiachao23/jcohy-study-sample/blob/master/jcohy-study-linux/images/1.jpg)

```shell
#查看密码策略的相关信息
show variables like '%password%';
```

validate_password_policy：密码策略，默认为MEDIUM策略
validate_password_dictionary_file：密码策略文件，策略为STRONG才需要
validate_password_length：密码最少长度
validate_password_mixed_case_count：大小写字符长度，至少1个
validate_password_number_count ：数字至少1个
validate_password_special_char_count：特殊字符至少1个
*上述参数是默认策略**MEDIUM**的密码检查规则*

```shell
set global validate_password_policy=0;
select @@validate_password_length;
set global validate_password_length=1;
```

11、添加远程登录用户

```shell
GRANT ALL PRIVILEGES ON *.* TO 'jiac'@'%' IDENTIFIED BY 'jiac0917!' WITH GRANT OPTION;
```

12、配置默认编码为utf8

```shell
#修改/etc/my.cnf配置文件，在[mysqld]下添加编码配置，如下所示：
[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'
```
13、开启端口访问

```shell
firewall-cmd --permanent --zone=public --add-port=3306/tcp
firewall-cmd --permanent --zone=public --add-port=3306/udp
firewall-cmd --reload
firewall-cmd --list-ports  
```
14、开启远程登录

```shell
CREATE USER 'root'@'%' IDENTIFIED BY 'jiac0917!';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
flush privileges;
```

## Redis


### 1、下载地址

https://pan.baidu.com/s/1iP5QDXSYHQ5q61cBFQvs-A

https://github.com/MSOpenTech/redis/releases

### 2、常规安装

1、[到官网](https://github.com/MSOpenTech/redis/releases)下载redis安装包

```shell
$ wget http://download.redis.io/releases/redis-5.0.5.tar.gz
$ tar xzf redis-5.0.5.tar.gz
$ cd redis-5.0.5
$ make
```

2、make完后 redis-2.8.17目录下会出现编译后的redis服务程序redis-server,还有用于测试的客户端程序redis-cli,两个程序位于安装目录 src 目录下：

下面启动redis服务.

```shell
$ cd src
$ ./redis-server
```

3、注意这种方式启动redis 使用的是默认配置。也可以通过启动参数告诉redis使用指定配置文件使用下面命令启动。

```shell
$ cd src
$ ./redis-server ../redis.conf
```

4、**redis.conf** 是一个默认的配置文件。我们可以根据需要使用自己的配置文件。

启动redis服务进程后，就可以使用测试客户端程序redis-cli和redis服务交互了。 比如：

```shell
$ cd src
$ ./redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```

### 3、docker安装

1、使用docker安装[redis](http://www.runoob.com/docker/docker-install-redis.html)

```shell
docker pull redis
mkdir -p /docker/redis/conf /docker/redis/data
docker run --restart=always --privileged=true -p 6379:6379 -v /docker/redis/data:/data -v /docker/redis/conf/redis.conf:/etc/redis/redis.conf --name myredis -d redis redis-server --appendonly yes

```

## nginx

### 1、下载地址：

http://nginx.org/en/linux_packages.html#RHEL-CentOS

http://software.jcohy.com/linux/nginx-1.6.2.tar.gz 

### 2、常规安装

1、安装编译工具及库文件

```shell
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel
```

2、首先要安装 PCRE

- 下载 PCRE 安装包，下载地址： <http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz>

```shell
[root@bogon src]# cd /opt/software
[root@bogon src]# wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz
```

- 解压安装包:

```shell
[root@bogon src]# tar zxvf pcre-8.35.tar.gz
```

- 进入安装包目录

```shell
[root@bogon src]# cd pcre-8.35
```

- 编译安装

```shell
[root@bogon pcre-8.35]# ./configure
[root@bogon pcre-8.35]# make && make install
```

- 查看pcre版本

```shell
[root@bogon pcre-8.35]# pcre-config --version
```

3、安装 Nginx

- 下载 Nginx，下载地址：<http://nginx.org/download/nginx-1.6.2.tar.gz>

```shell
[root@bogon src]# cd /opt/software
[root@bogon src]# wget http://nginx.org/download/nginx-1.6.2.tar.gz
```

- 安装包

```shell
[root@bogon src]# tar zxvf nginx-1.6.2.tar.gz
```

- 进入安装包目录

```shell
[root@bogon src]# cd nginx-1.6.2
```

- 编译安装

```shell
[root@bogon nginx-1.6.2]# ./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=//opt/software/pcre-8.35
[root@bogon nginx-1.6.2]# make && make install
```

- 查看nginx版本

```shell
[root@bogon nginx-1.6.2]# /usr/local/nginx/sbin/nginx -v
```

4、启动nginx

```shell
[root@bogon conf]# /usr/local/nginx/sbin/nginx
```

5、Nginx 其他命令

```shell
/usr/local/webserver/nginx/sbin/nginx -s reload            # 重新载入配置文件
/usr/local/webserver/nginx/sbin/nginx -s reopen            # 重启 Nginx
/usr/local/webserver/nginx/sbin/nginx -s stop              # 停止 Nginx
```

6、错误处理

```
src/core/ngx_murmurhash.c: In function ‘ngx_murmur_hash2’:

原因，是将警告当成了错误处理，打开/usr/local/nginx-1.6.2/objs/Makefile，
去掉CFLAGS中的 -Werror
再重新make
```

### 3、docker安装

1、建立Nginx文件目录

```shell
mkdir -p /docker/nginx
mkdir -p /docker/nginx/html
mkdir -p /docker/nginx/conf.d
mkdir -p /docker/nginx/log
```
2、拉取镜像
```shell
docker pull nginx
```
3、建立一个标准镜像
```shell
docker run -p 80:80 --name nginx -d nginx
```
4、复制nginx.conf文件到宿主机nginx，执行下面命令前，先确保路径是否是1步骤的目录下
```shell
docker cp nginx:/etc/nginx/nginx.conf /docker/nginx/nginx.conf
```
5、利用同样的办法把default.conf复制出来
```shell
docker cp nginx:/etc/nginx/conf.d/default.conf /docker/nginx/conf.d/default.conf
```
6、删除这个标准镜像
```shell
docker rm -f nginx
```
7、nginx映射多个端口，并挂载服务器配置文件
```shell
docker run --name nginx -itd -p 80:80 -p 8800:8800 \
-v /docker/nginx/html:/usr/share/nginx/html \
-v /docker/nginx/log:/var/log/nginx \
-v /docker/nginx/nginx.conf:/etc/nginx/nginx.conf:ro \
-v /docker/nginx/conf.d:/etc/nginx/conf.d --privileged=true -d nginx;
```

### 使用编译后软件包安装

这里本人将nginx软件进行了编译。使用编译后的文件直接解压使用即可。

https://software.jcohy.com/linux/nginx-1.6.2.build.tar.gz 

```
tar -zxvf nginx-1.6.2.build.tar.gz  -C /usr/local
```

## RabbitMq


### 1、下载地址

### 3、常规安装

1、由于RabbitMQ依赖Erlang， 所以需要先安装Erlang

```shell
  wget https://packages.erlang-solutions.com/erlang-solutions-1.0-1.noarch.rpm
  rpm -Uvh erlang-solutions-1.0-1.noarch.rpm
  yum install erlang
```

2、安装

```shell
  wget http://www.rabbitmq.com/releases/rabbitmq-server/v3.6.6/rabbitmq-server-3.6.6-1.el7.noarch.rpm
  yum install rabbitmq-server-3.6.6-1.el7.noarch.rpm
```

3、启动

```shell
  rabbitmq-server start
```

4、开启web管理接口

```shell
rabbitmq-plugins enable rabbitmq_management
```

5、通过浏览器访问

```shell
  http://localhost:15672
```

  6、其他命令

  ```shell
sudo chkconfig rabbitmq-server on  # 添加开机启动RabbitMQ服务
/sbin/service rabbitmq-server start # 启动服务
/sbin/service rabbitmq-server status  # 查看服务状态
/sbin/service rabbitmq-server stop   # 停止服务

# 查看当前所有用户
rabbitmqctl list_users

# 查看默认guest用户的权限
rabbitmqctl list_user_permissions guest

# 由于RabbitMQ默认的账号用户名和密码都是guest。为了安全起见, 先删掉默认用户
rabbitmqctl delete_user guest

# 添加新用户
rabbitmqctl add_user username password

# 设置用户tag
rabbitmqctl set_user_tags username administrator

# 赋予用户默认vhost的全部操作权限
rabbitmqctl set_permissions -p / username ".*" ".*" ".*"

# 查看用户的权限
rabbitmqctl list_user_permissions username

  ```

### 3、docker安装

	1、在docker官网查找docker镜像，https://hub.docker.com/
	
	2、拉取镜像,我们选择带有“mangement”的版本（包含web管理页面）

```shell
  docker pull rabbitmq:3.7.16-management

  docker run --restart=always -d -p 5672:5672 -p 15672:15672 --name myrabbitmq 3f92e6354d11
```

## git

```shell
yum -y install git-core
git --version
```

## gitlab

### 1、下载地址 

https://about.gitlab.com/install/#centos-7

### 2、常规安装

1、安装依赖

```shell
sudo yum install -y curl policycoreutils-python openssh-server
sudo systemctl enable sshd
sudo systemctl start sshd
sudo firewall-cmd --permanent --add-service=http
sudo systemctl reload firewalld


sudo yum install postfix
sudo systemctl enable postfix
sudo systemctl start postfix
```

2、下载源码并安装

```shell
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash

sudo EXTERNAL_URL="http://192.168.11.231:9090" yum install -y gitlab-ee
```

3、修改gitlab配置文件指定服务器ip和自定义端口

```
vi  /etc/gitlab/gitlab.rb

external_url -> 服务器http://IP:端口
```

4、执行配置

```
gitlab-ctl reconfigure
```

5、启动：gitlab-ctl start 出现如下说明成功

```
ok: run: alertmanager: (pid 7324) 0s 
ok: run: gitaly: (pid 7334) 0s 
ok: run: gitlab-monitor: (pid 7355) 1s 
ok: run: gitlab-workhorse: (pid 7360) 0s 
ok: run: logrotate: (pid 7373) 1s 
ok: run: nginx: (pid 7416) 0s 
ok: run: node-exporter: (pid 7461) 0s 
ok: run: postgres-exporter: (pid 7467) 1s 
ok: run: postgresql: (pid 7473) 0s 
ok: run: prometheus: (pid 7481) 1s 
ok: run: redis: (pid 7492) 0s 
ok: run: redis-exporter: (pid 7496) 1s 
ok: run: sidekiq: (pid 7502) 0s 
ok: run: unicorn: (pid 7517) 0s
```

6、修改储存位置

```
sudo vi /etc/gitlab/gitlab.rb 
git_data_dirs -> 修改成目标位置 
sudo gitlab-ctl reconfigure 配置执行 
重新启动
```


### 3、Docker安装中文版

https://docs.gitlab.com/omnibus/docker/

https://hub.docker.com/r/twang2218/gitlab-ce-zh

1、下载镜像

```shell
docker pull twang2218/gitlab-ce-zh
```

2、运行

```shell
docker run --detach \
  --hostname 192.168.11.238 \
  --publish 443:443 --publish 80:80 --publish 222:22 \
  --name gitlab \
  --restart always \
  --volume /opt/gitlab/config:/etc/gitlab  \
  --volume /opt/gitlab/logs:/var/log/gitlab \
  --volume /opt/gitlab/data:/var/opt/gitlab \
  twang2218/gitlab-ce-zh
  
  docker run -d \
    --hostname 192.168.11.238 \
    -p 80:80 \
    -p 443:443 \
    -p 222:22 \
    --privileged=true \
    --name gitlab \
    --restart always \
    -v /opt/gitlab/gitlab-config:/etc/gitlab \
    -v /opt/gitlab/gitlab-logs:/var/log/gitlab \
    -v /opt/gitlab/gitlab-data:/var/opt/gitlab \
   	twang2218/gitlab-ce-zh
```

3、数据存储

| **当地的位置**     | **docker位置**  | **用法**               |
| ------------------ | --------------- | ---------------------- |
| /opt/gitlab/config | /etc/gitlab     | 用于存储应用数据       |
| /opt/gitlab/logs   | /var/log/gitlab | 用于存储日志           |
| /srv/gitlab/config | /var/opt/gitlab | 用于存储GitLab配置文件 |

4、配置GitLab

```
docker exec -it gitlab /bin/bash
docker exec -it gitlab vi /etc/gitlab/gitlab.rb
修改external_url

sudo docker restart gitlab
```

5、查看log

```
docker logs -f gitlab
```

6、配置邮箱

```
gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.exmail.qq.com"
gitlab_rails['smtp_port'] = 465
gitlab_rails['smtp_user_name'] = "jia_chao23@126.com"
gitlab_rails['smtp_password'] = "Jia@1203"
gitlab_rails['smtp_authentication'] = "login"
gitlab_rails['smtp_enable_starttls_auto'] = true
gitlab_rails['smtp_tls'] = true
gitlab_rails['gitlab_email_from'] = 'jia_chao23@126.com'
gitlab_rails['smtp_domain'] = "exmail.qq.com"

```

7、常用操作

```
//外部操作
docker restart gitlab
docker stop gitlab
docker rm gitlab

//内部操作
//重新配置GitLab以使更改生效
gitlab-ctl reconfigure
gitlab-ctl restart
//验证是否正确配置了所有内容：
gitlab-rake gitlab:incoming_email:check
gitlab-rails console
Notify.test_email('jia_chao23@126.com', 'Message Subject', 'Message Body').deliver_now
```

## node

### 1、下载地址

http://software.jcohy.com/linux/node-v10.16.0.tar.gz 

http://nodejs.org/dist/v10.16.0/node-v10.16.0.tar.gz 

### 2、常规安装

1、下载并解压

```shell
// 下载
wget http://nodejs.org/dist/v10.16.0/node-v10.16.0.tar.gz   
//解压
tar -zxvf node-v10.16.0.tar.gz -C /usr/local/   
```

   2、编译

```shell
./configure --prefix=/usr/local/node/v10.16.0 
```

报错 C++ compiler too old, need g++ 4.9.4 or clang++ 3.4.2 (CXX=g++)

```shell
//1、装bzip2解压工具
yum -y install bzip2
//2、下载gcc包
wget https://ftp.gnu.org/gnu/gcc/gcc-7.3.0/gcc-7.3.0.tar.gz
//3、安装依赖，自动下载可能会慢。建议手动下载，然后复制到安装目录下。需要下载以下四个包。下载地址：ftp://gcc.gnu.org/pub/gcc/infrastructure/
gmp-6.1.0.tar.bz2: 
mpfr-3.1.4.tar.bz2: 
mpc-1.0.3.tar.gz: 
isl-0.16.1.tar.bz2: 

./contrib/download_prerequisites

//3、建立编译输出目录

mkdir gcc-build-7.3.0
//4、进入下面目录，执行命令，生成Makefile文件

../gcc-7.3.0/configure --enable-checking=release --enable-languages=c,c++ --disable-multilib
//5、执行命令进行编译，此处利用4个job，需编译时约40分钟，此值不宜设置过高

make -j4

//6 编译完成之后，需要把原来的GCC卸载，换成7.3.0版本的
rpm -q gcc 
rpm -q gcc-c++

删除已经安装低版本 
rpm -e [包名] 
包名为上一步中显示的包，形如： 
gcc-4.8.5-16.el7_4.2.x86_64 
gcc-c++-4.8.5-16.el7_4.2.x86_64

那就删除吧 
rpm -e gcc-4.8.5-16.el7_4.2.x86_64 
rpm -e gcc-c++-4.8.5-16.el7_4.2.x86_64


//7、安装 
make install

//8、验证版本
which gcc 
/usr/local/bin/gcc -v

//9、添加环境变量
//1、直接用export命令： 
export PATH=$PATH:/usr/local/bin 
查看是否已经设好，可用命令export查看，或者直接echo $PATH

//2、这个应该更靠谱一点，要不然有时候sudo的时候会提示找不到gcc命令 
sudo ln -s /usr/local/bin/gcc /usr/bin/gcc 
(建立/usr/bin/gcc到编译安装gcc的软链接)
```

报错：

```
node: /usr/lib64/libstdc++.so.6: version `GLIBCXX_3.4.21' not found (required by node)
node: /usr/lib64/libstdc++.so.6: version `GLIBCXX_3.4.15' not found (required by node)
node: /usr/lib64/libstdc++.so.6: version `GLIBCXX_3.4.20' not found (required by node)
```

`gcc`的动态库还是旧版本的。说明出现这些问题，是因为升级`gcc`时，生成的动态库没有替换老版本`gcc`的动态库。

```shell
//1、执行以下命令，查找编译gcc时生成的最新动态库：
find / -name "libstdc++.so*"
...
/opt/software/gcc-build-7.3.0/prev-x86_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6.0.24
/opt/software/gcc-build-7.3.0/prev-x86_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6
/opt/software/gcc-build-7.3.0/prev-x86_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so
/opt/software/gcc-build-7.3.0/x86_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6.0.24
...

//2、将上面的最新动态库libstdc++.so.6.0.21复制到/usr/lib64目录下：

cp /opt/software/gcc-build-7.3.0/x86_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6.0.24 /usr/lib64

//3、复制后，修改系统默认动态库的指向，即：重建默认库的软连接。
cd /usr/lib64
rm -rf libstdc++.so.6
ln -s libstdc++.so.6.0.21 libstdc++.so.6
```

### 3、淘宝镜像安装

```shell
.cd  /usr/loacl/node/  

wget https://npm.taobao.org/mirrors/node/v0.10.16/node-v0.10.16-linux-x64.tar.gz

tar -zxvf node-v0.10.16-linux-x64.tar.gz


ln -s /usr/local/node/node-v0.10.16-linux-x64/bin/npm /usr/local/bin/npm


ln -s /usr/local/node/node-v0.10.16-linux-x64/bin/node /usr/local/bin/node


npm -v
```

## yapi

1. 确保 node 版本=> 7.6,请运行 node -v 查看版本号
2. 确保 mongodb 版本 => 2.6，请运行 mongo --version 查看版本号
3. 确保安装了 npm, 运行 npm -v 查看版本号
4. 确保安装了 git,运行 git --version 查看版本号
5. 确保安装了 node-gyp 环境，[配置方法](https://github.com/nodejs/node-gyp#on-unix)

```shell
npm install -g yapi-cli --registry https://registry.npm.taobao.org
yapi server 
```

## mongodb



```
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.6.tgz

```

## docker

```shell
yum -y install docker
systemctl daemon-reload
systemctl restart docker.service
```

https://docs.docker.com/install/

报错：IPv4 forwarding is disabled. Networking will not work.

```shell
vim  /usr/lib/sysctl.d/00-system.conf
#添加下面的内容
net.ipv4.ip_forward=1
systemctl restart network && systemctl restart docker
```

## Jenkins

1、获取镜像

```
docker pull jenkinsci/blueocean
```

2、运行镜像

```
docker run \
-u root \
-d  \
-p 80:8080 \
-p 50000:50000 \
--privileged=true \
--name=jenkins \
-v /opt/jenkins/jenkins-data:/var/jenkins_home  \
-v /opt/jenkins/run/docker.sock:/var/run/docker.sock  \
jenkinsci/blueocean 
```

## 安装高版本gcc

1、下载高版本gcc，这里使用的是8.3.0

http://ftp.gnu.org/gnu/gcc/

编译后的gcc包，可解压后可直接从第5步开始：https://pan.baidu.com/s/159KLVjbaYALx6lrgibU-SQ

2、解压缩

```shell
tar -zxvf gcc-8.3.0.tar.gz -C /usr/local
yum install -y bzip2 glibc-headers gcc-c++ 
yum install -y glibc-headers
yum install -y gcc-c++ 
```

3、下载依赖包

```shell
cd /usr/local/gcc-8.3.0/
./contrib/download_prerequisites
```

4、编译+安装

```shell
mkdir ../gcc8.3.0build && cd ../gcc8.3.0build

../gcc-8.3.0/configure --prefix=/usr/local/gcc8.3.0build --enable-checking=release --enable-languages=c,c++ --disable-multilib
make && make install
```

5、之间已有低版本的gcc存在，先删除已建的软连接

```shell
rm -rf /usr/bin/gcc
rm -rf /usr/bin/g++
ln -s /usr/local/gcc8.3.0build/bin/gcc /usr/bin/gcc
ln -s /usr/local/gcc8.3.0build/bin/g++ /usr/bin/g++
```

6、查看新版本

```shell
gcc --version
```

7、运行程序时可能会出现/lib64/libstdc++.so.6: version  `GLIBCXX_3.4.20' not found，是因为升级安装了gcc，生成的动态库没有替换老版本的gcc动态库导致的。

查看包含最新的动态链接库的位置

```shell
find / -name "libstdc++.so*"
```

找到在/usr/local/gcc5/lib64/文件夹下

```shell
cp /usr/local/gcc8.3.0build/lib64/libstdc++.so.6.0.25  /usr/lib64/libstdc++.so.6.0.25
rm -f /usr/lib64/libstdc++.so.6
ln /usr/lib64/libstdc++.so.6.0.25 /usr/lib64/libstdc++.so.6

```

8、查看libstdc++.so.6链接包含的动态库

```shell
strings /usr/lib64/libstdc++.so.6|grep GLIBC
```

9、error 排除

-  C++ preprocessor "/lib/cpp" fails sanity check

```shell
yum install -y glibc-headers
yum install -y gcc-c++ 
```

## LDAP

1、安装LDAP

```shell
yum install -y openldap-servers openldap-clients
#拷贝数据库配置文件,DB_CONIFG中主要是关于Berkeley DB的相关的一些配置
cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG

systemctl start slapd
systemctl enable slapd
systemctl status slapd
```

2、**配置ldap服务**

```shell
#生成管理员密码
slappasswd
New password:
Re-enter new password:
{SSHA}krOGXDmiCdSXuXocOf10F96LJO5ijdXo  #记住这个,下面会用到
```

3、新建一个rootpwd.ldif(名称是自定义的)的文件:

```shell
vi rootpwd.ldif

dn: olcDatabase={0}config,cn=config
changetype: modify
add: olcRootPW
olcRootPW: {SSHA}krOGXDmiCdSXuXocOf10F96LJO5ijdXo

#ldif即LDAP Data Interchange Format，是LDAP中数据交换的一种文件格式。文件内容采用的是key-value形式，注意value后面不能有空格。
#上面内容中dn即distingush name
#olc即Online Configuration，表示写入LDAP后不需要重启即可生效
#changetype: modify表示修改一个entry，changetype的值可以是add,delete, modify等。
#add: olcRootPW表示对这个entry新增了一个olcRootPW的属性
#olcRootPW: {SSHA}krOGXDmiCdSXuXocOf10F96LJO5ijdXo指定了属性值
```

```shell
#下面使用ldapadd命令将上面的rootpwd.ldif文件写入LDAP:
ldapadd -Y EXTERNAL -H ldapi:/// -f rootpwd.ldif
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
modifying entry "olcDatabase={0}config,cn=config"
```

4、**导入schema**

导入schema，schema包含为了支持特殊场景相关的属性，可根据选择导入，这里先全部导入:

```shell
ls /etc/openldap/schema/*.ldif | while read f; do ldapadd -Y EXTERNAL -H ldapi:/// -f $f; done
```

5、**设定默认域**

```shell
# 先使用slappasswd生成一个密码:
slappasswd
New password:
Re-enter new password:
{SSHA}OpMcf0c+pEqFLZm3i+YiI2qhId1G/yM3
```

```shell
#新建一个domain.ldif的文件:

vi domain.ldif

dn: olcDatabase={1}monitor,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth"
  read by dn.base="cn=Manager,dc=jiac,dc=cn" read by * none

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=jiac,dc=cn

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=Manager,dc=jiac,dc=cn


dn: olcDatabase={2}hdb,cn=config
changetype: modify
add: olcRootPW
olcRootPW: {SSHA}OpMcf0c+pEqFLZm3i+YiI2qhId1G/yM3 #替换上面生成的密码


dn: olcDatabase={2}hdb,cn=config
changetype: modify
add: olcAccess
olcAccess: {0}to attrs=userPassword,shadowLastChange by
  dn="cn=Manager,dc=jiac,dc=cn" write by anonymous auth by self write by * none
olcAccess: {1}to dn.base="" by * read
olcAccess: {2}to * by dn="cn=Manager,dc=jiac,dc=cn" write by * read
```

- `olcAccess`即access，该key用于指定目录的ACL即谁有什么权限可以存取什么
- `olcRootDN`设定管理员root用户的distingush name
- 注意替换上面文件内容中cn为具体的域信息
- olcRootPW用上面新生成的密码替换

```shell
#写入
ldapmodify -Y EXTERNAL -H ldapi:/// -f domain.ldif
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
modifying entry "olcDatabase={1}monitor,cn=config"

modifying entry "olcDatabase={2}hdb,cn=config"

modifying entry "olcDatabase={2}hdb,cn=config"

modifying entry "olcDatabase={2}hdb,cn=config"

modifying entry "olcDatabase={2}hdb,cn=config"
```

6、**添加基本目录**

```shell
dn: dc=jiac,dc=cn
objectClass: top
objectClass: dcObject
objectclass: organization
o: jiac cn
dc: jiac

dn: cn=Manager,dc=jiac,dc=cn
objectClass: organizationalRole
cn: Manager
description: Directory Manager

dn: ou=People,dc=jiac,dc=cn
objectClass: organizationalUnit
ou: People

dn: ou=Group,dc=jiac,dc=cn
objectClass: organizationalUnit
ou: Group
```

- 注意替换上面文件内容中dn为具体的域信息
- 理解dn,cn,dc
  - DC即Domain Component，LDAP目录类似文件系统目录`dc=jiac,dc=cn`相当于`/cn/jiac`
  - CN即Common Name，CN有可能代表一个用户名，例如`cn=Manager,dc=jiac,dc=cn`表示在`/cn/jiac`域下的管理员用户Manager
  - OU即Organizational Unit，例如`ou=People,dc=jiac,dc=cn`表示在`/cn/jiac`域下的一个组织单元`People`

```shell
#写入:
ldapadd -x -D cn=Manager,dc=jiac,dc=cn -W -f basedomain.ldif
Enter LDAP Password:
adding new entry "dc=jiac,dc=cn"

adding new entry "cn=Manager,dc=jiac,dc=cn"

adding new entry "ou=People,dc=jiac,dc=cn"

adding new entry "ou=Group,dc=jiac,dc=cn"
```

7、测试

```shell
ldapsearch -LLL -W -x -D "cn=Manager,dc=jiac,dc=cn" -H ldap://localhost -b "dc=jiac,dc=cn"
Enter LDAP Password:
dn: dc=jiac,dc=cn
objectClass: top
objectClass: dcObject
objectClass: organization
o: jiac cn
dc: jiac

dn: cn=Manager,dc=jiac,dc=cn
objectClass: organizationalRole
cn: Manager
description: Directory Manager

dn: ou=People,dc=jiac,dc=cn
objectClass: organizationalUnit
ou: People

dn: ou=Group,dc=jiac,dc=cn
objectClass: organizationalUnit
ou: Group
```

8、可以在局域网内的windows电脑上下载[ldapadmin](http://www.ldapadmin.org/download/ldapadmin.html)作为管理工具

[basedomain.ldif](https://github.com/jiachao23/jcohy-study-sample/tree/master/jcohy-study-linux/bash/basedomain.ldif)

[domain.ldif](https://github.com/jiachao23/jcohy-study-sample/tree/master/jcohy-study-linux/bash/domain.ldif)

[rootpwd.ldif](https://github.com/jiachao23/jcohy-study-sample/tree/master/jcohy-study-linux/bash/rootpwd.ldif)

## Confluence

参考文档：

https://www.cnblogs.com/ios9/p/9045035.html#_label0

https://blog.csdn.net/LuckySuger/article/details/85121715

https://confluence.atlassian.com/confkb/known-issues-for-mysql-13138.html

https://www.cwiki.us/display/CONFLUENCEWIKI/Connecting+to+an+LDAP+Directory

1、下载，不同的版本

```shell
wget https://downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-6.12.1-x64.bin
wget https://downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-6.7.1-x64.bin
wget https://downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-6.9.1-x64.bin
wget https://downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-6.13.0-x64.bin
```

2、下载破解器

http://software.jcohy.com/windows/confluence%E7%A0%B4%E8%A7%A3%E5%B7%A5%E5%85%B7.zip 

3、安装confluence

```shell
cd /opt
chmod +x atlassian-confluence-6.12.1-x64.bin
./atlassian-confluence-6.12.1-x64.bin
```

4、破解jar包

下载破解和mysql驱动

https://software.jcohy.com/windows/confluence%E7%A0%B4%E8%A7%A3%E5%B7%A5%E5%85%B7.zip 

- 1、备份jar包

  ```shell
  mv /opt/atlassian/confluence/confluence/WEB-INF/lib/atlassian-extras-decoder-v2-3.4.1.jar /opt/atlassian-extras-2.4.jar
  ```

  

- 2、通过FTP将atlassian-extras-2.4.jar传到本地

- 3、运行破解器confluence_keygen.jar

  java -jar confluence_keygen.jar

![](D:\Document\picture\confluence_keygen1.png)

- 4、点击.patch，选择atlassian-extras-2.4.jar文件，点击打开，jar文件破解成功

- 5、上传破解后jar包到/opt/atlassian/confluence/confluence/WEB-INF/lib，并重命名atlassian-extras-decoder-v2-3.4.1.jar

- 6、上传mysql驱动/opt/atlassian/confluence/confluence/WEB-INF/lib

5、重启服务

```shell
chmod 777 atlassian-extras-decoder-v2-3.4.1.jar
service confluence stop；
service confluence start；
```

6、访问confluence

http://localhost:8090

7、点击产品安装

8、拷贝服务ID通过破解器获取key

9、设置外部数据库

mysql设置问题：

- 数据库字符编码

  ＆characterEncoding = utf8
  
  ```none
  utf8_bin
  ```
  
- 事务隔离级别 

	?sessionVariables=tx_isolation='READ-COMMITTED'

  ```none
  e.g. jdbc:mysql://127.0.0.1:3306/confdb?sessionVariables=tx_isolation='READ-COMMITTED'
  ```

- MySQL存储引擎

  修改启动配置文件：default-storage-engine=INNODB

10、配置邮箱服务器

![1565061967220](D:\Document\picture\confluence_keygen2.png)



11、Confluence修改数据库配置文件

第一步：是找到confluence的安装目录，我的安装目录在 /opt/atlassian/；

第二步：由于confluence把tomcat给改造了，所以confluence的应用并没有在workapps下，而是在confluence下面；

我们的工程的配置文件在classes文件夹，于是我找到了/opt/atlassian/confluence/confluence/WEB-INF/classes这个目录，

查看这个目录下面有一个confluence-init.properties文件，感觉像是大概的配置文件，打开这个文件，发现最后面有一行代码：confluence.home = /var/atlassian/application-data/confluence

第三步、进入confluence.home配置的文件夹，打开这个文件夹看到 有一个confluence.cfg.xml文件，打开这个文件，发现配置的数据库连接池一类的东西，真正的算是找到了，修改hibernate.connection.url的value为新的数据库地址 重新启动服务；

 /var/atlassian/application-data/confluence下confluence.cfg.xml文件：

## docker

### 1、系统要求
Docker 运行在 CentOS 7.X 之上 （不支持内核在 3.8 以下的老版本）
通过以下命令查看您的 CentOS 内核
```shell
uname -r
```
### 2、移除非官方软件包
```shell
 yum remove docker
```
### 3、卸载旧版本
```shell
yum remove docker \
                  docker-common \
                  docker-selinux \
                  docker-engine
```
### 4、设置 Yum 源
Docker 有多种安装方式，例如 Yum 安装、PRM 包安装、Shell安装等。以下以 Yum 安装方式进行, Docker 分 Docker EE 和 Docker CE 两种版本（EE：企业版，收费的；CE：社区版，不收费）。

* 1、安装 yum-utils , 这样就能使用 yum-config-manager 工具设置 Yum 源。
```shell
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
* 2、执行以下命令，添加 Docker 的 Yum 源。
```shell
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
* 3、【可选】启用测试仓库。测试仓库包含在 docker.repo 文件中，但默认情况下禁用的。
```shell
 yum-config-manager --enable docker-ce-edge
```
```shell
 yum-config-manager --enable docker-ce-test
```
* 4、如想要禁用测试仓库，可执行以下命令：
 ```shell
 yum-config-manager --disable docker-ce-edge
 ```
### 5、安装docker
```shell
yum makecache fast
//安装最新版本的 Docker
yum install docker-ce
//在生产系统中，可能需要安装指定版本的 Docker ，而并不总是安装最新的版本。执行以下命令，即可列出可用的 Docker 版本
yum list docker-ce --showduplicates | sort -r
//列出 Docker 版本后，可使用以下命令安装指定版本的 Docker 。
yum install
//例如：
yum -y install docker-ce-17.09.0.ce
```
### 6、启动docker
```shell
systemctl start docker
```
### 7、执行以下命令，验证安装是否正确
```shell
docker run hello-world
```
### 8、查看 Docker 版本
```shell
docker version
```
### 9、卸载docker
```shell
//卸载 Docker 软件包
yum remove docker-ce
//如需删除镜像、容器、卷以及自定义的配置文件，可执行以下命令
sudo rm -rf /var/lib/docker
```
### 10、设置docker镜像加速
国内访问 Docker Hub 的速度很不稳定，有时甚至出现连接不上的情况。为 Docker 配置镜像加速器，从而解决这个问题。目前国内很多云服务商都提供了镜像加速的服务。常用的镜像加速器有：
阿里云加速器、DaoCloud加速器等。
以阿里云加速器为例：
1、注册阿里云账号，即可在阿里云控制台（https://cr.console.aliyun.com/#/accelerator）
2、按照提示说明，即可配置镜像加速器。

### 11、ipv4转向配置
若启动docker的时候报错：WARNING IPv4 forwarding is disabled. Networking will not work
解决方案：
```shell
vi /etc/sysctl.conf
//添加配置
net.ipv4.ip_forward=1
//重启
systemctl restart network
//查看是否成功
sysctl net.ipv4.ip_forward

//如果返回为net.ipv4.ip_forward = 1则表示成功了
```

## docker-compose
### 1、二进制安装
```shell
//指定版本安装：
$ sudo curl -L https://get.daocloud.io/docker/compose/releases/download/1.23.1/\
docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

//对二进制文件赋可执行权限
$ sudo chmod +x /usr/local/bin/docker-compose
```
### 2、yum源安装
```shell
yum -y install docker-compose
```

### 3、测试安装是否成功
```shell
$ docker-compose --version
docker-compose version 1.23.1, build 1719ceb
```

## Harbor
注意：Harbor的所有服务组件都是在Docker中部署的，所以官方安装使用Docker-compose快速部署，所以我们需要安装Docker、Docker-compose。由于Harbor是基于Docker Registry V2版本，
所以就要求Docker版本不小于1.10.0，Docker-compose版本不小于1.6.0。

### 1、安装Harbor
```shell
mkdir /opt/software
cd /opt/software
wget https://storage.googleapis.com/harbor-releases/release-1.6.0/harbor-offline-installer-v1.6.0.tgz
tar xvzf harbor-offline-installer-v1.6.0.tgz -C /usr/local
cd /usr/local/harbor
vi harbor.cfg
hostname = 192.168.11.239 //自行更改为服务器内部ip
./install.sh
```
### 2、若需要修改配置
```shell
docker-compose down -v
vim harbor.cfg
prepare
docker-compose up -d
```
### 3、重启
```shell
docker-compose stop
docker-compose start
```
### 4、登录Harbor
> 地址：http://192.168.11.239
> 用户名：admin
> 密码：Harbor12345

### 5、测试推送

> 注意，此处需要结合第4步，登录到控制台后创建 aix 项目。

```shell
//1.admin登录
$ docker login 192.168.11.239
Username: admin
Password:
Login Succeeded

//2.给镜像打tag
$ docker tag nginx 192.168.11.239/aix/nginx:latest
$ docker images

//3.push到仓库
$ docker push 192.168.11.239/aix/nginx

```

### 6、开放2375端口
注意：在外网开放有安全风险，只推荐在内网对外开放

```shell
vi /lib/systemd/system/docker.service
//在[Service]节点下方增加
ExecStart=
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock
```
```shell
systemctl daemon-reload
systemctl restart docker
docker-compose stop
docker-compose start
firewall-cmd --add-port=2375/tcp --permanent
service firewalld restart
```

### 7、maven配置
```pom
<pluginGroups>
  <pluginGroup>com.spotify</pluginGroup>  
</pluginGroups>

<server>
  <id>192.168.11.239</id>
  <username>admin</username>
  <password>Harbor12345</password>
  <configuration>
    <email>smallchill@163.com</email>
  </configuration>
</server>
```

### 8、FAQ
1.配置并启动Harbor之后，本地执行登录操作，报错：
```shell
docker login 192.168.11.239
Username: admin
Password:
Error response from daemon: Get https://192.168.11.239/v1/users/: dial tcp 192.168.11.239:443: getsockopt: connection refused
```
这是因为docker1.3.2版本开始默认docker registry使用的是https，我们设置Harbor默认http方式，所以当执行用docker login、pull、push等命令操作非https的docker regsitry的时就会报错。

解决办法：配置/etc/docker/daemon.json
```shell
vi /etc/docker/daemon.json 

{
  "registry-mirrors": ["https://3dse7md.mirror.aliyuncs.com"]
}
//将其修改为：
{
  "registry-mirrors": ["https://3dse7md.mirror.aliyuncs.com"],
  "insecure-registries":["192.168.11.239"]
}

//重启
systemctl daemon-reload && systemctl restart docker
```
2.使用非admin账户执行docker pull命令时，报错
```shell
$ docker pull 192.168.11.239/aix/nginx:latest
Error response from daemon: repository 192.168.11.239/aix/nginx not found: does not exist or no pull access
```
这是原因可能有，一是Harbor仓库上aix/nginx确实不存在，二是项目未设置公开的时候，该账户未执行docker login 192.168.11.239 登录操作，三是该账户对192.168.11.239/aix项目没有权限，需要在该项目下增加aix成员，并选择角色。

3.如果需要修改Harbor的配置文件harbor.cfg，因为Harbor是基于docker-compose服务编排的，我们可以使用docker-compose命令重启Harbor。不修改配置文件，重启Harbor命令：docker-compose start | stop | restart
```shell
//停止Harbor
$ docker-compose down -v
Stopping nginx ... done
Stopping harbor-jobservice ... done
......
Removing harbor-log ... done
Removing network harbor_harbor

//启动Harbor
$ docker-compose up -d
Creating network "harbor_harbor" with the default driver
Creating harbor-log ... 
......
Creating nginx
Creating harbor-jobservice ... done
```

## 常用软件安装脚本

```shell
#!/bin/bash

BASE_URL=192.168.11.235/centos
BASE_DIR=/opt/software
JDK_PACKAGE=jdk-8u221-linux-x64.tar.gz
TOMCAT_PACKAGE=apache-tomcat-7.0.94.tar.gz
MYSQL_PACKAGE=mysql-8.0.16-2.el7.x86_64.rpm-bundle.tar
GCC_PACKAGE=gcc-8.3.0.tar.gz
NGINX_PACKAGE=nginx-1.6.2.build.tar.gz
#使用说明，用来提示输入参数
usage() {
	echo "如果需要安装 jdk，请执行  source ./install.sh jdk"
	echo "如果需要安装 tomcat，请执行  ./install.sh tomcat"
	echo "如果需要安装高版本的 gcc8，请执行  ./install.sh gcc8"
	echo "如果需要安装 redis，请执行  ./install.sh redis 。注意：此软件是以 docker 方式安装。"
	echo "如果需要安装 rabbitmq，请执行  ./install.sh rabbitmq 。注意：此软件是以 docker 方式安装。"
	echo "如果需要安装 nginx，请执行  ./install.sh nginx"
	echo "如果需要安装 sentinel，请执行  ./install.sh sentinel"
	echo "如果需要安装 docker，请执行  ./install.sh docker"
	echo "如果需要安装以上全部软件，请执行  ./install.sh all"
	exit 1
}

if  [ -x "$(command -v expect)" ];then
    echo 'expect 已经安装'
else
    echo 'expect  未安装,开始安装 expect 工具'
    #安装expect
    yum install -y expect
fi

if [ -x "$(command -v wget)" ];then
    echo 'wget 已经安装'
else
    echo 'wget 未安装,开始安装 wget 工具'
    #安装wget
    yum install -y wget
fi

if [ ! -d "$BASE_DIR" ]; then
	echo '创建目录。'$BASE_DIR
    mkdir $BASE_DIR
fi


installJDK(){
	if [ -x "$(command -v java)" ];then
		echo '====================== JDK 已存在,JAVA_HOME='$JAVA_HOME' ======================'
	else
		echo '====================== 开始安装 JDK ======================'
		wget -N $BASE_URL/$JDK_PACKAGE -P $BASE_DIR
		tar -zxvf $BASE_DIR/$JDK_PACKAGE -C /usr/local
		cat >> /etc/profile << EOF
export JAVA_HOME=/usr/local/jdk1.8.0_221
export PATH=\$PATH:\$JAVA_HOME/bin
EOF
		
		echo '====================== JDK 已安装完成，JAVA_HOME='$JAVA_HOME' ======================'
	fi
	
	
}

installNGINX(){
	if [ -d "$BASE_DIR/nginx" ];then
		echo '====================== NGINX 已存在 ======================'
	else
		echo '====================== 开始安装 NGINX ======================'
		wget -N $BASE_URL/$NGINX_PACKAGE -P $BASE_DIR
		tar -zxvf $BASE_DIR/$NGINX_PACKAGE -C /usr/local
		/usr/local/nginx/sbin/nginx
		pid = ps -ef|grep nginx | awk 'NR==1{ print $2 }'
		if [ ! $? -eq 0 ];then
		echo '====================== NGINX 已安装完成! ======================'
		else
		echo '====================== NGINX 安装失败!，请检查文件是否存在！ ======================'
		fi
		
	fi
}


function installGCC(){
	if [ -x "$(command -v gcc)" ];then
		echo 'GCC 已存在' 
	else
		echo '====================== 安装 gcc ======================'
		wget -N $BASE_URL/$GCC_PACKAGE -P $BASE_DIR
		tar -zxvf $BASE_DIR/$GCC_PACKAGE -C /usr/local
		cd /usr/local/gcc8.3.0build
		rm -rf /usr/bin/gcc
		rm -rf /usr/bin/g++
		ln -s /usr/local/gcc8.3.0build/bin/gcc /usr/bin/gcc
		ln -s /usr/local/gcc8.3.0build/bin/g++ /usr/bin/g++
		cp /usr/local/gcc8.3.0build/lib64/libstdc++.so.6.0.25  /usr/lib64/libstdc++.so.6.0.25
		rm -f /usr/lib64/libstdc++.so.6
		ln /usr/lib64/libstdc++.so.6.0.25 /usr/lib64/libstdc++.so.6
		echo '====================== GCC 安装完成 ======================'
		echo '====================== GCC 版本 ======================'
		gcc -v
		strings /usr/lib64/libstdc++.so.6|grep GLIBC
	fi
}


function installMYSQL(){

	if [ -x "$(command -v mysql)" ];then
		echo '======================mysql已存在======================' 
	else
		echo '======================安装mysql======================'
		wget -N $BASE_URL/$MYSQL_PACKAGE -P $BASE_DIR
		cd $BASE_DIR
		tar -xvf $BASE_DIR/$MYSQL_PACKAGE 
		echo '---------->>删除依赖'
		rpm -qa|grep mariadb-libs | xargs rpm -e  --nodeps
		#安装必要依赖
		echo '---------->>安装依赖项'
		yum install -y libaio net-tools perl numactl
		echo '---------->>安装Mysql'
		rpm -ivh mysql-community-common-8.0.16-2.el7.x86_64.rpm   
		rpm -ivh mysql-community-libs-8.0.16-2.el7.x86_64.rpm   
		rpm -ivh mysql-community-client-8.0.16-2.el7.x86_64.rpm  
		rpm -ivh mysql-community-server-8.0.16-2.el7.x86_64.rpm 
		echo '---------->>启动Mysql'
		systemctl enable mysqld
		systemctl start mysqld
		
		#pattern="^(?![A-Za-z0-9]+$)(?![a-z0-9\\W]+$)(?![A-Za-z\\W]+$)(?![A-Z0-9\\W]+$)[a-zA-Z0-9\\W]{8,}$"
		#while [[ "$newPassword" =~ $pattern ]]
		#do
		read -p '准备修改mysql密码，请输入新密码：新密码必须包含大小写字母、数字和特殊符号，并且长度不能少于8位。' newPassword
		#done
		# grep 'temporary password' /var/log/mysqld.log | sed -r 's/.*localhost: (.*)/\1/g'
		cd ~
		password=`grep 'temporary password' /var/log/mysqld.log|tail -n 1| awk '{print $NF}'`
		echo '---------->>mysql 默认密码：'$password
		./sql.sh $password $newPassword
		echo '====================== 开启端口访问======================'
		firewall-cmd --permanent --zone=public --add-port=3306/tcp
		firewall-cmd --permanent --zone=public --add-port=3306/udp
		firewall-cmd --reload
		firewall-cmd --list-ports 
		echo '====================== mysql 安装完成 Mysql密码为'$newPassword'======================'
	fi
}


function installTomcat(){
	echo '====================== 安装 tomcat ======================'
	#pid=ps -ef | grep "tomcat" | grep -v grep | awk '{print $2}'
	filename=${TOMCAT_PACKAGE%.tar.gz}
	unzipUrl=/usr/local/$filename
	if [ ! -d "$unzipUrl" ];then
		wget -N $BASE_URL/$TOMCAT_PACKAGE -P $BASE_DIR
		tar -zxvf $BASE_DIR/$TOMCAT_PACKAGE -C /usr/local/
		#/usr/local/apache-tomcat-7.0.94/bin/startup.sh
		echo '====================== tomcat安装完成 ======================'	
	else
		echo '====================== tomcat目录已存在 ======================'
		echo '====================== tomcat目录: ======================'$unzipUrl
	fi
	
}


function installDocker(){

	if ! [ -x "$(command -v docker)" ]; then
	   echo '---------->>检测到 Docker 尚未安装。正在试图从网络安装...所需时间与你的网络环境有关'
	   echo '---------->>安装一些必要的系统工具：'
	   yum install -y yum-utils device-mapper-persistent-data lvm2
	   echo '---------->>添加软件源信息：'
   	   yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
 	   yum makecache fast
	   yum -y install docker-ce
  	   systemctl start docker
	   systemctl enable docker
	  #echo 'It was detected that Docker was not installed. Attempting to install from the network... The time required is related to your network environment.'
	  #echo '检测到Docker尚未安装。正在试图从网络安装...所需时间与你的网络环境有关'
	  #curl -sSL https://get.daocloud.io/docker | sh
	  #chkconfig docker on 
	fi
	  #if ! [ -x "$(command -v docker)" ]; then
		#echo 'It was detected that Docker was not installed. Attempting to install from the network... The time required is related to your network environment.' 
		#echo '检测到Docker尚未安装。正在试图从网络安装...所需时间与你的网络环境有关'
		#curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -
		#chkconfig docker on 
	  #fi
	if ! [ -x "$(command -v docker)" ]; then
	  echo ' Docker 自动安装失败,建议你手动安装好 docker 环境后再启动本脚本' 
	  exit 1
	fi
	echo '---------->> docker 安装成功！'
}

function installRedis(){
	installDocker
	if  [  "$(docker images  |grep redis)" ]; then
	  echo "你已经安装过redis镜像"
	  exit 1 
	fi
	echo '====================== 开始安装 redis 镜像 ======================'
	docker pull redis
	mkdir -p /docker/redis/conf /docker/redis/data
	echo '------------->> 启动 redis 镜像'
	docker run --restart=always --privileged=true -p 6379:6379 -v /docker/redis/data:/data -v /docker/redis/conf/redis.conf:/etc/redis/redis.conf --name redis -d redis redis-server --appendonly yes
	if [ $? -eq 0 ]; then
		echo '====================== redis 安装启动成功 ======================'
		echo "====================== redis 配置文件：/docker/redis/ ======================"
	else
		echo "====================== redis 启动失败！详情参考 docker logs redis ======================"
		exit 1
	fi
}
function installSentinel(){
	installDocker
	if  [  "$(docker images  |grep sentinel)" ]; then
	  echo "你已经安装过sentinel镜像"
	  exit 1 
	fi
	echo '====================== 开始安装 sentinel 镜像 ======================'
	docker pull bladex/sentinel-dashboard
	echo '------------->> 启动 sentinel 镜像'
	docker run --restart=always --name sentinel -d -p 8858:8858 -d bladex/sentinel-dashboard
	if [ $? -eq 0 ]; then
		echo '======================  sentinel 镜像启动成功！ ======================'
	else
		echo '======================  sentinel 启动失败！详情参考 docker logs sentinel ======================'
		exit 1
	fi
}
function installRabbitmq(){
	installDocker
	if  [  "$(docker images  |grep rabbitmq)" ]; then
	  echo "你已经安装过rabbitmq镜像"
	  exit 1 
	fi
	echo '====================== 开始安装 rabbitmq 镜像 ======================'
	docker pull rabbitmq:3.7.16-management
	echo '------------->> 启动 rabbitmq 镜像'
	docker run --restart=always -d -p 5672:5672 -p 15672:15672 --name rabbitmq 3f92e6354d11
	if [ $? -eq 0 ]; then
		echo '======================  rabbitmq 镜像启动成功！ ======================'
	else
		echo '======================  rabbitmq 启动失败！详情参考 docker logs rabbitmq ======================'
		exit 1
	fi
}

function removeMysql(){
	rpm -e mysql-community-server-8.0.16-2.el7.x86_64
	rpm -e mysql-community-client-8.0.16-2.el7.x86_64 
	rpm -e mysql-community-libs-8.0.16-2.el7.x86_64
	rpm -e mysql-community-common-8.0.16-2.el7.x86_64
	   
	   
	#groupdel mysql
	userdel mysql

	rm -rf /var/lib/mysql
	rm -rf /var/lib/mysql/mysql
	rm -rf /var/log/mysqld.log

}
#根据输入参数，选择执行对应方法，不输入则执行使用说明
case "$1" in
"jdk")
	installJDK
;;
"tomcat")
	installTomcat
;;
"mysql")
	installMYSQL
;;
"gcc8")
	installGCC
;;
"redis")
	installRedis
;;
"rabbitmq")
	installRabbitmq
;;
"nginx")
	installNGINX
;;
"sentinel")
	installSentinel
;;
"docker")
	installDocker
;;
"removeMysql")
	removeMysql
;;
"all")
	installJDK
	installTomcat
	installMYSQL
	installGCC
	installRedis
	installRabbitmq
	installNGINX
	installDocker
;;
*)
	usage
;;
esac

```
