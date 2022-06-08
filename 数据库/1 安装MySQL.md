[toc]



# 安装Mysql

- 检查是否已经安装， 是则删除

```shell
 sudo netstat -tap | grep mysql // 输入后需要等几秒钟，如果没有反应就是没有
```

**开始安装**：

1. 安装
   
   ```shell
   sudo apt-get install mysql-server mysql-client 
   sudo apt install libmysqlclient-dev
   ```
   
2. 检查是否安装成功：

   ```shell
   sudo netstat -tap | grep mysql // 需要等一会儿才会有内容
   或者通过登录测试：
   mysql -uroot -p
   ```

## 设置

权限设置：

```shell
sudo chown -R mysql:mysql /var/lib/mysql
```

初始化：

`sudo mysqld --initialize`

启动mysql：

`sudo systemctl start mysqld`   // 本宫使用的是`sudo systemctl start mysql`

- 如果出现错误 `Unit mysqld.service not found` 见下面

查看MySQL运行状态：

`systemctl status mysql`

```shell
 /var/run  systemctl status mysql
● mysql.service - MySQL Community Server
     Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2021-09-23 10:32:54 CST; 31s ago
    Process: 75629 ExecStartPre=/usr/share/mysql/mysql-systemd-start pre (code=exited, status=0/SUC>
   Main PID: 75637 (mysqld)
     Status: "Server is operational"
      Tasks: 38 (limit: 11848)
     Memory: 353.7M
     CGroup: /system.slice/mysql.service
             └─75637 /usr/sbin/mysqld

9月 23 10:32:53 yang-virtual-machine systemd[1]: Starting MySQL Community Server...
9月 23 10:32:54 yang-virtual-machine systemd[1]: Started MySQL Community Server.

```



## 验证运行是否正常

**mysqladmin检查mysql服务器版本：**

```bash
mysqladmin --version
```



**使用mysql Client执行简单的命令**：

1. 进入mysql：`sudo mysql`

2. 以上命令执行后会输出 mysql>提示符，这说明你已经成功连接到Mysql服务器上，你可以在 mysql> 提示符执行SQL命令：

   ```go
   mysql> SHOW DATABASES;
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | mysql              |
   | performance_schema |
   | sys                |
   +--------------------+
   4 rows in set (0.01 sec)
   ```

   



## 创建密码、链接服务器

**设置密码** ： `mysqladmin -u root password "new_password";` （默认密码为空）

**连接到服务器**： `mysql -u root -p` 之后会提示你输入密码

# reference

[设置参考:菜鸟教程](https://www.runoob.com/mysql/mysql-install.html)

[安装参考](https://www.jianshu.com/p/cc01f7773346)

## 遇到的问题





启动MySQL出现错误：

`Failed to start mysqld.service: Unit mysqld.service not found`

解决办法：服务名错误。将mysqld 改为mysql 。 成功。

> 如果还是没有成功则按照其他教程（其他方法）

参考： [Unit mysqld.service](https://blog.csdn.net/qq_31083947/article/details/90248565)





**错误**：

`mysqld_safe Directory '/var/run/mysqld' for UNIX socket file don't exists.`

```shell
mkdir -p /var/run/mysqld
chown mysql:mysql /var/run/mysqld
```



[stackflow](https://stackoverflow.com/questions/42153059/mysqld-safe-directory-var-run-mysqld-for-unix-socket-file-dont-exists)

