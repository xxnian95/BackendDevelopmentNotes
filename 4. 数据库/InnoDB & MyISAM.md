# InnoDB & MyISAM
#4. 数据库/引擎#
- - - -
## 是否支持行级锁
MyISAM 只有表级锁(table-level locking)，而InnoDB 支持行级锁(row-level locking)和表级锁,默认为行级锁。

## 是否支持事务和崩溃后的安全恢复
**MyISAM** 强调的是性能，每次查询具有原子性,其执行数度比InnoDB类型更快，但是不提供事务支持。但是**InnoDB** 提供事务支持事务，外部键等高级数据库功能。 具有事务(commit)、回滚(rollback)和崩溃修复能力(crash recovery capabilities)的事务安全(transaction-safe (ACID compliant))型表。

## 是否支持外键
MyISAM不支持，而InnoDB支持。
**外键**：引用另一个表中的一列或多列数据，用来建立或者加强两个表之间的联系。

## 是否支持MVCC
仅 InnoDB 支持（**InnoDB是事务安全的**）。应对高并发事务, MVCC比单纯的加锁更高效;MVCC只在READ_COMMITTED和REPEATABLE_READ两个隔离级别下工作;MVCC可以使用 乐观(optimistic)锁 和 悲观(pessimistic)锁来实现;各数据库中MVCC实现并不统一。

## 其他
1. MyIASM支持支持**全文索引**，InnoDB不支持。
2. MyIASM保存了**表的行数**，InnoDB没有保存。
3. MyIASM一般情况下效率更高，适合小型应用。一般的Web应用应当首选MyIASM
## 应用场景
* MySIAM适合需要执行大量`SELECT`的应用场景。
* InnoDB适合需要执行大量`INSERT`和`UPDATE`操作的应用场景，可以提高**多用户并发操作**的性能。

