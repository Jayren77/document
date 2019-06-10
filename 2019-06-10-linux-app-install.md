
### 一、虚拟机

#### 第一步基本就是安装虚拟机

本地使用的vm的破解版，没有什么难度，直接正常安装即可，因为需要联网，这里在网卡的选择上使用的是桥接模式

除此之外没有其他的配置，网上出现的问题这里暂时没有，如果有我觉得可以试着把桥接网络的子网地址修改为与主机一个网段（不知道这里的理解对不对），总之这一步没有出现什么问题，不喜欢用Centos，有些问题不是很好解决，还是ubuntu熟悉一些，下面说一些软件的安装，虚拟机还是用来折腾的，所以下面就说一下Mysql，Hadoop，ELK，Logstash等软件环境的安装。

### 二、Mysql

安装参考 http://wiki.ubuntu.org.cn/MySQL

#### 第一步当然是下载了

运行命令：

```bash
sudo apt-get install mysql-server mysql-client
```

等待..
网址上说还要下一个web界面用来管理，就不需要了，本地用Navicat就可以..

#### 第二步检查服务是否启动

当然了，一般安装好了以后就直接启动了
运行命令：

```bash
pgrep mysqld
```

#### 第三步进入Mysql完成一些基本配置

首先登陆就出现了问题
运行命令：

```bash
mysql -u root -p
```

但是我并没有密码
度娘以后，找到一个[解决办法](https://blog.csdn.net/theonegis/article/details/51810063)

1. 查看这个配置文件 ``/etc/mysql/debian.cnf``，这是自动生成的

```bash
host     = localhost
user     = debian-sys-maint
password = Pveo2ji1L1YJnGD7
socket   = /var/run/mysqld/mysqld.sock
[mysql_upgrade]
host     = localhost
user     = debian-sys-maint
password = Pveo2ji1L1YJnGD7
socket   = /var/run/mysqld/mysqld.sock
```

这里有一个自动生成的用户
下面简单说一下Mysql的配置``debian-sys-maint``
使用它登录mysql.

2. 登录后运行命令：
```
update user set authentication_string=PASSWORD("root") where User='root';
update user set plugin="mysql_native_password";
flush privileges;
```

3. 重启服务，再登录，搞定；

#### 第四步解除一下远程连接的IP限制

1. 运行命令：

```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

注释掉这段（centos安装后不在这个目录）

2. root用户登录，更新mysql数据库中的user表；


运行命令：

`` use mysql``


``update user set host = '%' where user ='root' limit 1;``

刷新缓存：``flush privileges;``

重启服务；

主机测试连接，成功。mysql的安装就完成了。

下面是一些Mysql的配置

1、配置文件：/etc/mysql/my.cnf 这是一个全局配置，个性配置可以以xxx.my.cnf来完成

2、默认的数据库文件存储位置：datadir = /var/lib/mysql 仅mysql用户可以进入。

### 三、安装Hadoop(TODO)

### 四、安装Redis

Redis 的安装暂时就觉得就是一句话, `sudo apt-install redis`

### 五、安装Go环境

ubuntu下也是一句话: `sudo apt install golang-go`

### 六、安装open-falcon

官方的教程上面是使用的centos吧,反正用的是yum管理器,但是我用不来,所以本机上使用的是ubuntu,这里的记录也是根据这个[官方教程](http://book.open-falcon.org/zh_0_2/quick_install/frontend.html)
这里使用的最新版v0.2
下面记录一下中间的坑:

#### 后端搭建

当按照要求完成了环境准备,安装redis/mysql/搭建go环境,基本apt-get搞定,不赘述了
这里说一句,如果是单机开发,就不要配密码了,简单root/""

#### 前端搭建

由于是依赖了python包的,ubuntu下部分软件的安装有区别,这里记录修改一下:
1. python-virtualenv  直接apt-get
2. python-devel 这个不一样

```bash
sudo apt-get install aptitude
sudo aptitude install python-dev
```

具体的可以参考:https://zhidao.baidu.com/question/984886308187248259.html
3. openldap-devel 这个暂时我没有安装,看了下教程,很麻烦,试了一下,并无报错,以后有问题再增加.
4. mysql-devel 查了下没有这个东西,百度后找到一条命令:
apt-get install libmysqld-dev libmysqlclient-dev
5 后面的内容依照教程逐步启动即可.
**补充一下**：github上找到的ubuntu环境下的依赖软件包：

```bash
apt-get install -y python-virtualenv
apt-get install -y slapd ldap-utils
apt-get install -y libmysqld-dev
apt-get install -y build-essential
apt-get install -y python-dev libldap2-dev libsasl2-dev libssl-dev

cd $HOME/open-falcon/dashboard/
virtualenv ./env

./env/bin/pip install -r pip_requirements.txt -i https://pypi.douban.com/simple
```

### 七、Docker(Centos7)安装

### 八、触摸板管理工具

一个触摸板管理工具，包括可以禁用指点干（键盘上的小圆点），在ubuntu18.04上测试可用。

```bash
sudo add-apt-repository ppa:atareao/atareao
sudo apt-get update
sudo apt-get install touchpad-indicator 
```
甚至可以开机即启动。

### 九、 Maven 安装


