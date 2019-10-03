# MySQL语句

## 关键字
SQL语言包括数据定义(DDL)、数据操纵(DML)、数据控制(DCL)和数据查询（DQL）四个部分。
- 数据定义：`CREATE TABLE`、`ALTER TABLE`、`DROP TABLE`、 `CRAETE/DROP INDEX`等
- 数据操纵：`SELECT` 、`INSERT`、`UPDATE`、`DELETE`、
- 数据控制：`GRANT`、`REVOKE`
- 数据查询：`SELECT`

## 显示前50行
```sql
SELECT * FROM <tablename> 
LIMIT 0, 50;
```
