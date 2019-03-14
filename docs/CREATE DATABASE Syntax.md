# 创建数据库的基本语法：
```sql
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name
    [create_specification] ...

create_specification:
    [DEFAULT] CHARACTER SET [=] charset_name
  | [DEFAULT] COLLATE [=] collation_name
  | DEFAULT ENCRYPTION [=] {'Y' | 'N'}
```
- DATABASE 和 SCHEMA 是同义词，两者都可以使用，效果一致。

- IF NOT EXISTS 代表如果创建的数据库存在的话不报错。

- CHARACTER SET 和 COLLATE 作为编码格式和排序格式可以在初始创建时指定，否则按配置默认。

- ENCRYPTION 是 MYSQL8.0 新有的特性，直译为“加密”，Y 代表数据库和将来创建的表都会进行加密，N 代表不加密。

需要注意的是，以往可以通过在 data 目录下新建一个文件夹实现创建数据库这个功能。在MYSQL8.0禁止了这种操作。

# 示例
这是创建DATABASE的基本结构。作为示例这里创建一个名为 test 的数据库。
```shell
mysql> CREATE DATABASE test;
ERROR 1007 (HY000): Can't create database 'test'; database exists
```

因为我之前创建过名为 test 的数据库，所以系统会报错。
```shell
mysql> CREATE DATABASE IF NOT EXISTS test;
Query OK, 1 row affected, 1 warning (0.07 sec)

mysql> SHOW warnings;
+-------+------+-----------------------------------------------+
| Level | Code | Message                                       |
+-------+------+-----------------------------------------------+
| Note  | 1007 | Can't create database 'test'; database exists |
+-------+------+-----------------------------------------------+
1 row in set (0.00 sec)
```

如果加上IF NOT EXISTS语句系统就不会报错，而是以一个warnings的形式提醒。

我们再来看看创建一个不存在的数据库是怎样的：
```sql
mysql> CREATE DATABASE test1;
Query OK, 1 row affected (0.20 sec)
```
可以看到这次没有任何错误和异常提醒。
