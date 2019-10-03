# MySQL语句

## 关键字
SQL语言包括数据定义(DDL)、数据操纵(DML)、数据控制(DCL)和数据查询（DQL）四个部分。
- 数据定义：`CREATE TABLE`、`ALTER TABLE`、`DROP TABLE`、 `CRAETE/DROP INDEX`等
- 数据操纵：`SELECT` 、`INSERT`、`UPDATE`、`DELETE`、
- 数据控制：`GRANT`、`REVOKE`
- 数据查询：`SELECT`

## 限制
refman-2029
检索第5行
`SELECT * FROM table LIMIT 5;`
检索记录行6-15
`SELECT * FROM table LIMIT 5,10;`

## 排序
排序的列可以不是显示的列
```sql
SELECT column1, column2, …
FROM table
ORDER BY column3, column 4, …;
```

## 降序
```sql
SELECT column1, …
FROM table
ORDER BY column2 DESC;
```

涉及到多个列的排序，其中某些是降序
```sql
SELECT column1, …
FROM table
ORDER BY column2 DESC, column 3;
```
DESC只能直接作用于前面的column名。

`FROM`、`ORDER BY`、`LIMIT`的顺序：
```sql
FROM … 
ORDER BY … 
LIMIT …
```
