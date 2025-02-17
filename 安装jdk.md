



## 安装jdk

1. 安装 JDK 前，一定确保提前删除了虚拟机自带的 JDK。

[root@hadoop100 ~]# rpm -qa | grep -i java | xargs -n1 rpm -e --nodeps



2.下载这个

![72235103144](C:\Users\19125\AppData\Local\Temp\1722351031446.png)

3 .赋予用户写权限：

> chmod 757 文件夹

4. 解压安装包 ：

> tar xzf jdk-22_linux-x64_bin.tar.gz 



5. 执行命令：  vim  /etc/profile  写环境变量

~~~
export JAVA_HOME=/usr/local/jdk-22.0.2

export PATH=$JAVA_HOME/bin:$PATH

export CLASSPATH=$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
~~~

6. 输入java -version验证是否安装成功![72235143128](C:\Users\19125\AppData\Local\Temp\1722351431286.png)


参考：https://blog.csdn.net/weixin_51414096/article/details/126274479







## 安装Python

1.centos7 自带了Python 2.7.5版本

![72242337161](C:\Users\19125\AppData\Local\Temp\1722423371616.png)

不用管，我们可以直接安装Python3，最后切换版本即可

2.安装依赖包

~~~
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel

~~~

3. 访问Python的安装包网址为：<https://www.python.org/ftp/python/3.12.4/

![72242565991](C:\Users\19125\AppData\Local\Temp\1722425659913.png)



4.  切换目录 ，我们下载 .tgz 文件即可「Windows则安装exe程序，mac安装pkg」，下载命令为：

```
cd /usr/local 
wget https://www.python.org/ftp/python/3.12.4/Python-3.12.4.tgz
```

5.解压。切换到解压后的目录。编译。安装

~~~
tar -zxvf Python-3.12.4.tgz

cd Python-3.12.4
./configure --prefix=/usr/local/python3
make && make install

~~~



6.建立软链接

1）软连接命令

~~~
ln -s /usr/local/python3/bin/python3.12 /usr/bin/python
ln -s /usr/local/python3/bin/pip3.12 /usr/bin/pip
~~~

`注意这里是3.12`        安装包版本是3.12.4



2)软命令说明

① 确定python和pip的运行位置

~~~
whereis python
whereis pip
~~~

cd 到查询出的命令路径下，查看对应的python和pip的软连接状态：

~~~
cd /usr/bin

ls -l python
ls -l pip
~~~

7.运行python，查看是否可用



参考：https://blog.csdn.net/qq_42571592/article/details/122902266







## git 安装

1.卸载已有的git

~~~
sudo yum -y remove git
sudo yum -y remove git-*
~~~

2.下载git

~~~
cd /usr/local
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.46.0.tar.gz

~~~

3.解压。切换到解压后的目录。编译。安装

~~~
tar -zxvf git-2.46.0.tar.gz
cd git-2.46.0
./configure --prefix=/usr/local/git
make && make install
~~~

4.配置环境变量

~~~
 vim /etc/profile
 
 export PATH=/usr/local/git/bin:$PATH

~~~

5.查看git版本

```
 git --version
```

参考： https://blog.csdn.net/xwj1992930/article/details/96428998

https://blog.csdn.net/sxau_zhangtao/article/details/129789497





## ngnix 安装

1.卸载已有的nginx

~~~
rm -rf  /usr/bin/nginx
~~~



2.切换目录  下载安装包

~~~
cd /usr/local
wget https://nginx.org/download/nginx-1.26.1.tar.gz
~~~

3.解压。切换到解压后的目录。配置。编译。安装

~~~
tar -zxvf nginx-1.26.1.tar.gz
cd nginx-1.26.1
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
make && make install
~~~



4. 为了方便后续快速[启动nginx](https://so.csdn.net/so/search?q=%E5%90%AF%E5%8A%A8nginx&spm=1001.2101.3001.7020)，可以给nginx配置环境变量

~~~
vim /etc/profile
export PATH=$PATH:/usr/local/nginx/sbin

~~~

5.让配置生效

~~~
source /etc/profile
~~~



6.直接输入`nginx`启动服务



7.查看是否运行

~~~
#查看是否运行
netstat -ntlp
~~~



![72243660775](C:\Users\19125\AppData\Local\Temp\1722436607754.png)



8.防火墙设置

注意： 由于firewall使用的是python2，当前系统被修改默认环境为python3。

问题描述：

![72243667029](C:\Users\19125\AppData\Local\Temp\1722436670290.png)

~~~
第一步，
vim /usr/bin/firewall-cmd， 将#！/usr/bin/python -Es 改为 #！/usr/bin/python2 -Es（到目前为止，上面提到的问题已解决）

第二步，
vim /usr/sbin/firewalld, 将#！/usr/bin/python -Es 改为 #！/usr/bin/python2 -Es (这一步是针对于防火墙报错，进行的修改)

~~~



9.开始检测防火墙是否开启

~~~
#检查防火墙是否开启
systemctl status firewalld
~~~

出现了running说明已经开启了防火墙，

10.开放端口号

~~~
#设置80端口开启 nginx默认端口号，如果修改了端口号就需要开放对应的端口号
firewall-cmd --zone=public --add-port=80/tcp --permanent

#执行防火墙相关操作立即生效 
firewall-cmd --reload

#查询开启的所有端口
firewall-cmd --list-port
~~~



11. 输入`ifconfig` 查到centos 7的ip, 在浏览器中输入 http://192.168.52.131:80  即可

![72243633182](C:\Users\19125\AppData\Local\Temp\1722436331821.png)



## node.js 安装

1.删除node.node_modules文件夹

~~~
rm -rf /usr/local/lib/node*
rm -rf  /usr/local/include/node*
rm -rf /usr/local/node*

# 查找并删除~文件夹里的node和node_modules文件夹
find ~/ -name node
find ~/ -name node_modules

# 使用rm -rf 命令删除对应结果


# 删除node可执行文件
rm -rf /usr/local/bin/npm
rm -rf /usr/local/share/man/man1/node.1
rm -rf  /usr/local/lib/dtrace/node.d
rm -rf ~/.npm
rm -rf /usr/bin/node
rm -rf /usr/bin/npm

~~~

2.下载

~~~~
# 下载解压nodejs
wget https://nodejs.org/dist/v20.16.0/node-v20.16.0-linux-x64.tar.gz  
tar -zxvf node-v20.16.0-linux-x64.tar.gz

~~~~

3.建立软链接

~~~
# 建立软链接
ln -s /usr/local/node-v20.16.0-linux-x64/bin/node /usr/bin/node

ln -s /usr/local/node-v20.16.0-linux-x64/bin/npm /usr/bin/npm


~~~

4.查看版本

~~~

node -v
npm -v

~~~

node安装失败。 https://blog.csdn.net/weixin_40861707/article/details/109455582

#node -v后报错 。是这个问题



5.查看版本，出现以下问题。需要将GLIBC升级到2.28

![72260534685](C:\Users\19125\AppData\Local\Temp\1722605346855.png)

6.安装所需要的glibc-2.28

6.1.先查看本机有没有gcc，没有就去安装

参考：https://www.cnblogs.com/niceyoo/p/14532228.html

6.2 再升级

~~~
wget http://ftp.gnu.org/gnu/glibc/glibc-2.28.tar.gz
tar xf glibc-2.28.tar.gz 
cd glibc-2.28/ && mkdir build  && cd build
../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin

~~~



7.安装这一步会出错......   需要升级gcc与make

7.1.升级gcc 4.8  -> 8.3.1

~~~
# 安装centos-release-scl 、 devtoolset
sudo yum install centos-release-scl
sudo yum install devtoolset-8-gcc*

# 直接替换旧的gcc
mv /usr/bin/gcc /usr/bin/gcc-4.8.5
ln -s /opt/rh/devtoolset-8/root/bin/gcc /usr/bin/gcc
mv /usr/bin/g++ /usr/bin/g++-4.8.5
ln -s /opt/rh/devtoolset-8/root/bin/g++ /usr/bin/g++
gcc --version

g++ --version

~~~

参考： https://www.cnblogs.com/jixiaohua/p/11732225.html



7.2.升级make

7.11.先确认是否已安装make

~~~
make 
~~~

7.22.本机未安装，所以先安装make

~~~
sudo yum install make

# 查看
make --version
~~~

7.23.升级make

~~~
# 升级 make(默认为3 升级为4)
wget http://ftp.gnu.org/gnu/make/make-4.3.tar.gz
tar -xzvf make-4.3.tar.gz && cd make-4.3/
./configure  --prefix=/usr/local/make

make && make install
cd /usr/bin/ && mv make make.bak
ln -sv /usr/local/make/bin/make /usr/bin/make
~~~

8.这时 所有的问题  解决完毕.。继续安装glibc-2.28

~~~
cd /usr/local/glibc-2.28/build
../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin

# 编译和安装
make && make install

~~~

9. 执行node -v 依次出现问题

   ![72260608421](C:\Users\19125\AppData\Local\Temp\1722606084216.png)

10.更新lib libstdc++.so.6.0.26

~~~
wget https://cdn.frostbelt.cn/software/libstdc%2B%2B.so.6.0.26

cp libstdc++.so.6.0.26 /usr/lib64/

cd /usr/lib64/

ln -snf ./libstdc++.so.6.0.26 libstdc++.so.6
验证

node -v
v20.15.0
~~~

参考：https://blog.csdn.net/qq_39287495/article/details/139964001



本机没有make就直接去升级，会出现下面的报错：

执行命令 less config.log 查看到 make命令未安装

![72260356775](C:\Users\19125\AppData\Local\Temp\1722603567750.png)

所以先安装make

~~~
sudo yum install make

# 查看
make --version
~~~

再次执行 ./configure  --prefix=/usr/local/make 后成功





## yum 报错 完美解决

前提 ：基本的网络配置好：

```
vi /etc/sysconfig/network-scripts/ifcfg-ens33

# 把no改为yes
ONBOOT=yes

# 在文件末尾追加
DNS1=8.8.8.8
DNS2=4.2.2.2


```

重启网络：

```
systemctl restart network.service
```



这他妈不说谁知道 操   https://wiki.bafangwy.com/doc/719/



上面还连接不上的话，再试试这个： https://developer.aliyun.com/article/1575375









## 终端打不开

![72265563919](C:\Users\19125\AppData\Local\Temp\1722655639194.png)





## mysql 安装



1.进入 官网 找到自己的版本 进行 下载安装包 ：        http://repo.mysql.com

![72266257029](C:\Users\19125\AppData\Local\Temp\1722662570290.png)

2.将下载 好的 安装包 上传到centos 7，的/usr/local目录

~~~
下载后拖到centos 7的桌面
mv ./mysql57-community-release-el7.rpm /usr/local
~~~

3.解压

~~~~
rpm -ivh mysql57-community-release-el7.rpm
~~~~

4.安装

~~~
yum install -y mysql-community-server
~~~

**可能会出现** **GPG** **密钥过期**

- 解决方案：输入一下命令，更新GPG 密钥

```
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
```

重新开始安装，再次执行 yum install -y mysql-community-server

5.第一次登录 调整配置文件，采用无密码登录

```
vim /etc/my.cnf  

# 再最后一行增加
skip-grant-tables
```

6.启动

~~~~
systemctl start mysqld

# 登录
mysql -uroot -p
~~~~

7. 为了 防止后期 ，MySQL  出现编码问题 ，我们需要对编码，进行配置

~~~
vim /etc/my.cnf   // 进入MySQL 的配置文件

# 加在中间即可。  端口、编码编制
port=3306
character-set-server=utf8
default-storage-engine=innodb
~~~

参考：https://blog.csdn.net/weixin_45031801/article/details/139429231



8.修改登录密码

~~~
1.先进入mysql

use mysql

mysql> update user set authentication_string=passworD("1234") where user='root';

flush privileges;

exit

vim /etc/my.cnf，删除 skip-grant-tables ，保存退出

# 重启
systemctl restart mysqld.service 
~~~

在登陆mysql后使用命令例如：show databases;  

首次登录还需重置密码， 此时需要重置你刚才设置的密码

~~~
mysql> set global validate_password_policy=0;

mysql> set global validate_password_length=3;

mysql> alter user 'root'@'localhost' identified by '1234';
~~~

参考：https://blog.csdn.net/weixin_42070473/article/details/107898031





9.更新MySQL的远程访问：

~~~
# 登录
mysql -u root -

use mysql；

#你想让root用户，密码为mypassword的用户 从任何主机连接到mysql服务器的话。
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '1234' WITH GRANT OPTION;
~~~

参考：https://developer.aliyun.com/article/1143774



10.打开centos主机的 iptables 3306端口

~~~~

$ iptables -I INPUT 4 -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT

$ service iptables save #保存iptables规则
~~~~

利用navicat即可连接了!

参考：https://www.cnblogs.com/xiadongqing/p/16062385.html



11.批量导入sql文件

把所有sql文件放在新建的data_sql文件夹下。在data_sql平级目录下创建 sql.sh 文件

~~~
# sql.sh

#!/bin/bash
dir=`ls /usr/local/data_sql/` #定义遍历的目录，这个是你sql的存放路径
echo "" > all.sql  #创建一个总的sql文件，注意别跟你现有的重名即可！
for i in $dir
do
    echo "source /usr/local/data_sql/$i;" >> all.sql
done 
~~~

执行sql.sh，得到all.sql文件

然后我们登录到mysql中，用source导入sql即可

```
mysql > source /usr/local/all.sql;

```

参考： https://www.cnblogs.com/lyc94620/p/11825371.html







## Centos 7防火墙相关命令

~~~
# 开启防火墙
systemctl start firewalld

# 关闭防火墙
systemctl stop firewalld

# 重启防火墙
systemctl restart firewalld

# 查看防火墙状态
systemctl status firewalld

# 开启端口
firewall-cmd --zone=public --add-port=80/tcp --permanent

# 关闭端口
firewall-cmd --zone=public --remove-port=80/tcp --permanent

# 查看开放的端口
firewall-cmd --list-ports

~~~



## nginx命令

~~~
# 启动 Nginx 服务器：
nginx：直接运行此命令即可启动 Nginx 服务器。

# 停止 Nginx 服务器：
nginx -s stop：快速停止 Nginx，不等待当前请求处理完毕。

nginx -s quit：优雅地停止 Nginx，等待当前所有请求处理完毕后再停止。

# 重新加载配置文件：
nginx -s reload：重新加载 Nginx 配置文件，不停止 Nginx 服务，这种方式可以避免服务中断。

# 查看 Nginx 版本：
nginx -v 或 nginx -V：显示 Nginx 的版本信息。
~~~



## 查看占用了特定端口的进程

~~~
ss -tnlp | grep 8081
~~~



## web前后端放到虚拟机中

12.1 前端访问后端的ip应该为虚拟机的ip地址，我把虚拟机ip设置为静态ip,每次都是固定的

12.2 虚拟机防火墙应该开放前端访问后端的端口









问题：

1.移动端无法访问虚拟机web前端

2.移动端可以访问虚拟机web前端，但后端接受不到参数

3.访问主机ip映射不到web了。



## 解决移动端可以访问虚拟机web前端，但后端接受不到参数的问题

1.利用VM中的 桥接模式

①虚拟机操作系统直接连接物理网卡，通过物理网卡与外部网络建立连接关系。
②此模式下，虚拟机与宿主机处于同一网段中，以独立IP的身份参与网络交互。与宿主机处于同一网段中的其他PC可以直接访问虚拟机（可以ping通虚拟机的IP，访问虚拟机上部署的应用）。

说白了桥接模式就是相当于一台独立的机器，可以直接接受外部请求

而NAT把宿主机当做一台网关，把对宿主机ip的访问转为 对虚拟机的访问。



2.查看宿主机的网关，子网掩码，及ip

![72275387745](C:\Users\19125\Desktop\2024-2月面试\job准备\Linux\安装jdk.assets\1722753877455.png)

3.修改虚拟机网络配置

~~~
vi /etc/sysconfig/network-scripts/ifcfg-ens33



IPADDR=192.168.1.138
NETMASK=255.255.255.0
GATAWAY=192.168.1.1

DNS1=114.114.114.114
DNS2=8.8.8.8

# 重启网络
service network restart

~~~

4. 可以在手机上 访问  192.168.1.138:80  ，并且后端可以收到请求！


参考：https://blog.csdn.net/hualinger/article/details/131188141


