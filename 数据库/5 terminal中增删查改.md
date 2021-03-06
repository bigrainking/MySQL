[toc]



增删查改

## terminal中数据库中操作



#### 新建表：

`CREATE TABLE 	'tablename'()`

```bash
mysql> CREATE TABLE `userdetail`(
    -> `uid` INT(10) NOT NULL DEFAULT `0`,
    -> `intro` TEXT NULL,
    -> `profile` TEXT NULL,
    -> PRIMARY KEY (`uid`)
    -> );
```



#### 删除表：

DROP TABLE  tablename；

```bash
mysql> DROP TABLE names;
Query OK, 0 rows affected (0.01 sec)
```



#### 3. 插入数据：

```bash
INSERT INTO table_name ( field1, field2,...fieldN ) #列名
                       VALUES
                       ( value1, value2,...valueN ); #值
```



```bash
mysql> INSERT INTO runoob_tbl 
    -> (runoob_title, runoob_author, submission_date)
    -> VALUES
    -> ("学习 PHP", "菜鸟教程", NOW()); #数据需要双引号
```



GO插入数据：

```go
func insertData(db *sql.DB) {
	// 返回值准备之后的查询and命令来使用。
	stmt, err := db.Prepare("INSERT INTO userinfo SET username=?,department=?,created=?")
	checkerr(err)

	// 执行语句
	res, err := stmt.Exec("肥猪之家", "幼儿园", "2020-02-09")
	checkerr(err)

	//返回一个数据库回应的命令的数字， 一般是一个自增数列的数字
	// 比如本table中有UID是自增的， 则目前增加的这一条数据对应的UID是8，则返回的id是8
	id, err := res.LastInsertId()
	checkerr(err)
	fmt.Println(id)
}
```



#### 4. 查询数据：

```bash
SELECT column_name,column_name
FROM table_name
[WHERE Clause]
[LIMIT N][ OFFSET M]
```

- column_name,column_name ： 用* 表示所有列
- LIMIT N ： 用来限制查询到的条数的数目



```
mysql> SELECT * FROM fun;
+-------+
| names |
+-------+
| babo  |
+-------+
1 row in set (0.00 sec)
```



