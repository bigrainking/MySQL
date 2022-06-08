count(1)固定数提高效率，但都是计算符合条件的条目有多少条

下面两个都是返回student表里有多少条数据

```sql
select count(1) from student ;
select count(*) from student ;
```

