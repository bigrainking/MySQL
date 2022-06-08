[toc]



## 一、启动关闭MySQL服务器

**启动**

```shell
启动：sudo service mysql start
查看运行状况：sudo service mysql status
关闭：sudo service mysql stop
```



查看是否启动成功：

`ps -ef | grep mysql` 查看mysql是否在运行





## 二、添加新用户



为mysql添加新的用户

`CREATE USER 'username'@'localhost' IDENTIFIED BY 'password'`

- 'username' : 替换成用户名， password替换为设置的密码
- @'localhost'：表明用户可以在哪里登录，@'localhost'表示在本地可以登录， @%表示任意machine登录，还可以设置远程登录

示例：

```shell
mysql> CREATE USER 'user1'@'localhost' IDENTIFIED BY 'user1';
Query OK, 0 rows affected (0.01 sec)
```

​	

### 改变用户权限

为上面新添加的用户设置权限。

权限有下面几种：

- **All Privileges:** The user account has full access to the database
- **Insert:** The user can insert rows into tables
- **Delete:** The user can remove rows from tables
- **Create:** The user can [create entirely new tables](https://phoenixnap.com/kb/how-to-create-a-table-in-mysql) and databases
- **Drop:** The user can drop (remove) entire tables and databases
- **Select:** The user gets access to the select command, to read the information in the databases
- **Update:** The user can update table rows
- **Grant Option:** The user can modify other user account privileges



**语法：**

> 注意mysql 8.0.11之后不需要后面的密码

```shell
GRANT permission_type ON database.table TO username@"localhost";
```

- permission_type:是对应的权限类型
- database.table：限制用户只能操作某个数据库对应的某个表 `*.*`表示所有数据库所有表

**示例**

```shell
mysql> GRANT INSERT,SELECT,DELETE,UPDATE,CREATE,DROP ON *.* TO zara@"localhost";
Query OK, 0 rows affected (0.02 sec)
```

下面是设置仅仅对一个表有INSERT权限的示例：

`GRANT INSERT *database_name.table_name* TO 'username'@'localhost';`



#### 显示用户信息

```shell
mysql>use mysql; #首先先进入mysql数据库
mysql> SELECT host, user, authentication_string FROM user WHERE user = 'guest';
+-----------+---------+------------------+
| host      | user    | password         |
+-----------+---------+------------------+
| localhost | guest | 6f8c114b58f2ce9e |
+-----------+---------+------------------+
1 row in set (0.00 sec)
```

- authentication_string： 以前的password现在已经改名为 `authentication_string`
- WHERE user = 'guest'： 查询名为guest的用户信息；



## 三、管理新用户



#### 登录进入终端：

- 必须在root用户下进入

```shell
[root@host]# mysql -u root -p   
Enter password:******  # 登录后进入终端(默认密码为空)
```

如果不在root用户下进入

```shell
 ✘ yang@VM  ~/Downloads  mysql -u root -p                      
Enter password: 
ERROR 1698 (28000): Access denied for user 'root'@'localhost'
```



**使用root用户进入数据库：**

root用户可以对数据库进行删除增加操作

```shell
[root@host]# mysqladmin -u root -p <操作>
Enter password:******
```

#### 退出终端：

exit

```
mysql> exit
Bye
```



#### 显示用户的权限

语法：`SHOW GRANTS [FOR user]`

示例：

```shell
mysql> SHOW GRANTS FOR 'guest'@'localhost';
+--------------------------------------------+
| Grants for guest@localhost                 |
+--------------------------------------------+
| GRANT INSERT ON *.* TO `guest`@`localhost` |
+--------------------------------------------+
1 row in set (0.00 sec)
```



#### 赋予一个用户操作某个数据库的所有权限

**赋予某个数据库的所有权限：**

`GRANT ALL PRIVILEGES ON database_name.* TO 'guest'@'localhost'`

赋予对应某个表：

`GRANT ALL PRIVILEGES ON *.* TO 'guest'@'localhost'`



#### 撤销用户某个权限





