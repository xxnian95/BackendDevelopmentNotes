# MySQL的字符串

字符串类型有：
1. SET
2. BLOB
3. ENUM
4. CHAR
5. TEXT

## BLOG、TEXT
BLOB是一个二进制对象，可以容纳可变数量的数据。TEXT是一个不区分大小写的BLOB。
BLOB和TEXT类型之间的::唯一区别::在于对BLOB值进行排序和比较时区分大小写，对TEXT值不区分大小写。