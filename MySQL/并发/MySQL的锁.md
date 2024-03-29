# MySQL的锁
[mysql数据库的锁有多少种，怎么编写加锁的sql语句][1]
## 锁定机制
1. **表级锁**：开销小，加锁快；不会出现死锁；锁定粒度大，发生锁冲突的概率最高，并发度最低；
2. **行级锁**：开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度也最高；
3. **页面锁**：开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般。
**适用情形**：从锁的角度来说，表级锁更适合于以*查询*为主，只有少量按索引条件更新数据的应用，如Web应用；而行级锁则更适合于*有大量按索引条件*并发更新少量不同数据，同时又有并发查询的应用，如一些在线事务处理（OLTP）系统。
---
## 表级锁
MySQL的表级锁有两种模式：表共享读锁（Table Read Lock）和表独占写锁（Table Write Lock）。锁模式的兼容性：
1. 对MyISAM表的读操作，不会阻塞其他用户对同一表的读请求，但会阻塞对同一表的写请求；
	2. 对MyISAM表的写操作，则会阻塞其他用户对同一表的读和写操作。
MyISAM表的读操作与写操作之间，以及写操作之间是串行的。当一个线程获得对一个表的写锁后，只有持有锁的线程可以对表进行更新操作。其他线程的读、写操作都会等待，直到锁被释放为止。
MyISAM在执行*查询语句（SELECT）*前，会自动给涉及的所有表加读锁，在执行*更新操作（UPDATE、DELETE、INSERT等）*前，会自动给涉及的表加写锁，这个过程并不需要用户干预，因此，用户一般不需要直接用`LOCK TABLE`命令给MyISAM表显式加锁。
查看表级锁的使用情况：
```sql
mysql> show status like 'table%';
+----------------------------+---------+
| Variable_name              | Value   |
+----------------------------+---------+
| Table_locks_immediate      | 100     |
| Table_locks_waited         | 11      |
+----------------------------+---------+
```
`Table_locks_immediate`：产生表级锁定的次数。
`Table_locks_waited`：出现表级锁定争用而发生等待的次数。
对于InnoDB表，在绝大部分情况下都应该使用行级锁，因为事务和行锁往往是我们之所以选择InnoDB表的理由。但在个别特殊事务中，也可以考虑使用表级锁：
1. 事务需要 _更新大部分或全部数据，表又比较大_ ，如果使用默认的行锁，不仅这个事务执行效率低，而且可能造成其他事务长时间锁等待和锁冲突，这种情况下可以考虑使用表锁来提高该事务的执行速度。
2. _事务涉及多个表_ ，比较复杂，很可能引起死锁，造成大量事务回滚。这种情况也可以考虑一次性锁定事务涉及的表，从而避免死锁、减少数据库因事务回滚带来的开销。
---
## 行级锁
InnoDB的行级锁定同样分为两种类型，共享锁和排他锁，而在锁定机制的实现过程中为了让行级锁定和表级锁定共存，InnoDB也同样使用了意向锁（表级锁定）的概念，也就有了意向共享锁和意向排他锁这两种。
当一个事务需要给自己需要的某个资源加锁的时候，如果遇到一个共享锁正锁定着自己需要的资源的时候，自己可以再加一个共享锁，不过不能加排他锁。但是，如果遇到自己需要锁定的资源已经被一个排他锁占有之后，则只能等待该锁定释放资源之后自己才能获取锁定资源并添加自己的锁定。而意向锁的作用就是当一个事务在需要获取资源锁定的时候，如果遇到自己需要的资源已经被排他锁占用的时候，该事务可以需要锁定行的表上面添加一个合适的意向锁。如果自己需要一个共享锁，那么就在表上面添加一个意向共享锁。而如果自己需要的是某行（或者某些行）上面添加一个排他锁的话，则先在表上面添加一个意向排他锁。意向共享锁可以同时并存多个，但是意向排他锁同时只能有一个存在。所以，可以说InnoDB的锁定模式实际上可以分为四种：共享锁（S），排他锁（X），意向共享锁（IS）和意向排他锁（IX），我们可以通过以下表格来总结上面这四种所的共存逻辑关系：
[image:0950C774-E8CE-4863-B402-22C92E104967-10586-0000AE7EFC02746E/1033231-20170118181153984-1417117507.png]()
如果一个事务请求的锁模式与当前的锁兼容，InnoDB就将请求的锁授予该事务；反之，如果两者不兼容，该事务就要等待锁释放。
::意向锁::是InnoDB自动加的，不需用户干预。对于UPDATE、DELETE和INSERT语句，InnoDB会自动给涉及数据集加排他锁（X)；对于普通SELECT语句，InnoDB不会加任何锁；事务可以通过以下语句显示给记录集加共享锁或排他锁。
```sql
共享锁（S）：SELECT * FROM table_name WHERE … LOCK IN SHARE MODE
排他锁（X)：SELECT * FROM table_name WHERE … FOR UPDATE
```
间隙锁也是一种行级锁。
查看行锁的竞争情况：
```sql
mysql> show status like 'InnoDB_row_lock%';
+-------------------------------+-------+
| Variable_name                 | Value |
+-------------------------------+-------+
| InnoDB_row_lock_current_waits | 0     |
| InnoDB_row_lock_time          | 0     |
| InnoDB_row_lock_time_avg      | 0     |
| InnoDB_row_lock_time_max      | 0     |
| InnoDB_row_lock_waits         | 0     |
+-------------------------------+-------+
```
1. `InnoDB_row_lock_current_waits`：当前正在等待锁定的数量；
	2. `InnoDB_row_lock_time`：从系统启动到现在锁定总时间长度；
	3. `InnoDB_row_lock_time_avg`：每次等待所花平均时间；
	4. `InnoDB_row_lock_time_max`：从系统启动到现在等待最常的一次所花的时间；
	5. `InnoDB_row_lock_waits`：系统启动后到现在总共等待的次数。

[1]:	https://www.cnblogs.com/sessionbest/articles/8689071.html
