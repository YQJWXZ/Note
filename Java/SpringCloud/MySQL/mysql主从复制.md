## MySQL主从复制

MySQL主从复制是一个异步的复制过程，底层是基于Mysql数据库自带的*二进制日志*功能。就是一台或多台MySQL数据库(slave, 即从库)
从另一台MySQL数据库(master, 即主库)进行日志的复制然后再进行解析日志并应用到自身，最终实现从库的数据和住哭的数据保持一致。

1. MySQL复制过程分为三步：
* master将改变记录到二进制日志(binary log)
* slave将master的binary log拷贝到它的中继日志(relay log)
* slave重做中继日志中的事件，将改变应用到自己的数据库中

2. 主从复制配置
* 配置-主库Master
```
1. 修改Mysql数据库的配置文件 /etc/my.cnf
[mysqld]
log-bin=mysql-bin  #启用二进制日志
server-id=100  #服务器唯一id

2. 重启Mysql服务

3. 登录数据库进行授权：建立复制时需要用到的用户权限，也就是slave必须被master授权具有该权限的用户，才能通过该用户复制
grant replication slave on *.* to 'username'@'%'identified by 'password';

4. 查看master的状态
show master status;
```
* 配置-从库slave
```
1. 修改Mysql数据库的配置文件 /etc/my.cnf
[mysqld]
server-id=101  #服务器唯一id

2. 重启Mysql服务

3. 登录数据库，执行sql
change master to master_host='192.168.138.100', master_user='username', master_password='password', 
master_log_file='mysql-bin.00001', master_log_pos=439;

start slave;

5. 查看从库状态
show slave status;
```