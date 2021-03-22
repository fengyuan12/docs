## 下载yum源
```javascript
wget 'https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm'
```
<img src='./img/linux_mysql_01.png'>

## 安装yum源
```javascript
rpm -Uvh mysql57-community-release-el7-11.noarch.rpm
```
<img src='./img/linux_mysql_02.png'>

## 查看有哪些版本的mysql
```javascript
yum repolist all | grep mysql
```
<img src='./img/linux_mysql_03.png'>

## 启用与禁用mysql版本
  默认安装启用的版本
```javascript
禁用 yum-config-manager --disable mysql80-community
启用 yum-config-manager --enable mysql80-community
```
>备注：<br/>
yum-config-manager 如果找不到命令  执行下面的命令<br/>
yum -y install yum-utils

## 安装mysql
```javascript
yum install -y mysql-community-server
```
<img src='./img/linux_mysql_04.png'>

## 启动mysql服务 及 查看mysql服务状态
```javascript
启动 systemctl start mysqld
查看 systemctl status mysqld
```
<img src='./img/linux_mysql_05.png'>

## 登录数据库，修改数据库密码
  找到密码: 红框的地方就是密码
```javascript
grep 'temporary password' /var/log/mysqld.log
```
<img src='./img/linux_mysql_06.png'>
### 进入mysql
```javascript
mysql -uroot -p
```
### 修改密码
```javascript
SET PASSWORD = PASSWORD('Admin123!');
```

## 设置远程可以登录
```javascript
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'Admin123!' WITH GRANT OPTION;
flush privileges;
```
<img src='./img/linux_mysql_07.png'>

## 修改一些简单的配置
```html
mysql的配置文件真的很多，有的还很蛋疼。比如默认的字符集是拉丁字符集，
每次创建数据库的时候要设置字符集；默认还不支持group by语句，
默认的时区也不是我们现在的北京时间(东八区)，会导致我们的时间差了13个点。
针对以上说几个简要的配置，更多的配置在以后遇到了再加上，或者留言吧！
先输入exit退出数据库客户端。
打开配置文件，yum安装的默认在/etc文件夹下:
vim /etc/my.cnf
在[mysqld]下面添加，不需要分号
字符集:注意是utf8而不是utf-8!
character-set-server=utf8
这时候使用show variables like 'char%';就可以查看到字符集都是utf8了
sql支持group by语句
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
设置时区为东八区
default-time_zone = '+8:00'
```
```javascript
最后重启数据库，使配置生效。
systemctl restart mysqld
```

## 设置开机启动
```javascript
systemctl enable mysqld
systemctl daemon-reload
```

## 本地连接虚拟机数据库
<em>一、先在本地ping虚拟机的ip是否能可以ping通<br/>二、cmd下执行telnet 192.168.217.129 3306 查看虚拟机的3306端口是否开放<br/>三、如果telent 无法连接<br/>四、在虚拟中开放防火墙3306端口</em>
```javascript
firewall-cmd --list-ports // 查看防火墙端口
firewall-cmd --zone=public --add-port=3306/tcp --permanent  // 开启3306端口
firewall-cmd --reload // 重启防火墙
```
<em>五、虚拟机数据库创建用户并附有所有权限</em>
```javascript
create user 'hhmed'@'%' identified by 'hhmed';
grant all on *.* to 'hhmed'@'%';
flush privileges;
```

>ps：<br/>
一般mysql是不允许除了本机用户以外的用户进行访问的,所以需要给特定ip的用户开放权限，通过这个用户去访问连接