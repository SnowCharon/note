# Linux

## 安装软件及配置环境

### 配置文件

```
~/.bashrc
/etc/profile
```

**每次更改配置还要刷新一下：```source filename```** 

冲突可能是因为配置文件配置的版本不同导致的

### 防火墙

查看状态：```systemctl status firewalld```

停止防火墙：```systemctl stop firewalld```

永久关闭：```systemctl disable firewalld```

重启防火墙：```systemctl enable firewalld```

开放端口：```firewall-cmd --reload```  ```firewall-cmd --zone=public --add-port=80/tcp --permanent```



### Redis

安装：[Redis](https://github.com/tporadowski/redis)

端口号：6379

启动：```./redis-server```

连接：```./redis-cli```

带配置文件启动：```src/redis-server ./redis.conf```

windows远程连接：```.\redis-cli.exe -h <ip> -p 6379 -a <password>```

#### Redis五种主要数据类型

- 字符串 String
- 哈希 hash
- 列表 list
- 集合 set
- 有序集合 sorted set

### Nginx

下载在官方下载，解压后还没完

cd nginxm后```./configure --prefix=/usr/local/nginx```,然后```make && make install```

在sbin目录下```./nginx -v```,查看版本号，```./nginx -t```查看配置文件是否正确

启动：```./nginx```

停止：```./nginx -s stop```\

重新加载配置文件：```./nginx -s reload```

### MySQL

#### 远程连接

1. ```
    update user set host = '%' where user = 'root';
    
    select host, user from user;
    ```

2. ```
    flush privileges;
    ```

### 安装

```tar -xvf mysql-server_8.0.30-1ubuntu20.04_amd64.deb-bundle_5.tar```

```sudo dpkg-preconfigure mysql-community-server_*.deb```

 ```sudo dpkg -i mysql-{common,community-client,community-client-core,community-client-plugins,client,community-server,server}_*.deb```

``` apt --fix-broken install```

``` sudo dpkg -i mysql-{common,community-client,community-client-core,community-client-plugins,client,community-server,server}_*.deb```

```sudo dpkg -i  libmecab2*.deb```



## 命令速记

后台运行：nohup <进程name> &> <logfilename> &

杀死进程：kill -9 <进程id>

查看运行的任务：ps -ef

筛选：ps -ef | grep <任务名>

更改文件权限：chmod 777 <filename>

打开设置：gnome-control-center

## 自动部署项目脚本

1. Maven仓库的配置一定要写好
2. 在执行脚本前先看看有没有正在执行的项目，它可能会占用端口资源影响项目启动
3. 项目在Maven打包后一定要查看一下打包的名称和脚本所执行的包的名称是否一致
4. git拉取的冲突需要注意

项目第一次运行会有些慢





## MySQL主从复制

blog:[MySQL主从复制_张三疯学独孤九剑的博客-CSDN博客](https://blog.csdn.net/main_Scanner01/article/details/124259050)

以下为在Ubuntu20.04系统操作经验：

**每次修改配置文件都要systemctl restart mysql;**

### 主库

mysql的配置文件要在：```/etc/mysql/mysql.conf.d/mysqld.cnf```中修改，加入server-id和log-bin的文件名

![image-20230224133812967](picture/image-20230224133812967.png)

操作：

*在主机MySQL里执行授权主从复制的命令 mysql8*
CREATE USER 'slave1'@'%' IDENTIFIED BY '123456';

GRANT REPLICATION SLAVE ON *.* TO 'slave1'@'%';

*此语句必须执行。否则见下面。*
ALTER USER 'slave1'@'%' IDENTIFIED WITH mysql_native_password BY '123456';

*刷新权限*

flush privileges;

*查询Master的状态，并记录下File和Position的值。*

show master status;

### 从库

mysql的配置文件要在：```/etc/mysql/mysql.conf.d/mysqld.cnf```中修改，加入server-id即可——不能和主库相同

**如果是克隆出来的系统，还要记得修改uuid,在```/var/lib/mysql/auto.cnf```中修改**——UUID可以通过在数据库中```select uuid();```函数获取

然后重启数据库后连接，分别执行：

1. ```CHANGE MASTER TO MASTER_HOST='192.168.213.128',MASTER_USER='tingxue',MASTER_PASSWORD='W1281409960',MASTER_LOG_FILE='mysql-bin.000007',MASTER_LOG_POS=157;```

    ```
    CHANGE MASTER TO
    MASTER_HOST='主机的IP地址',
    MASTER_USER='主机用户名',
    MASTER_PASSWORD='主机用户名的密码',
    MASTER_LOG_FILE='mysql-bin.具体数字',
    MASTER_LOG_POS=具体值;
    ```

2. start slave;

3. SHOW SLAVE STATUS\G;

### 重新配置主从

#### 停止同步

stop slave;

#### 重新配置主从，需要在从机上执行：

stop slave;
reset master; [[删除Master中所有的binglog文件，并将日志索引文件清空，重新开始所有新的日志文件]](慎用)



CHANGE MASTER TO MASTER_HOST='192.168.213.128',MASTER_USER='tingxue',MASTER_PASSWORD='W1281409960',MASTER_LOG_FILE='mysql-bin.000004',MASTER_LOG_POS=157;
