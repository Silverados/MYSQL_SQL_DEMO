# 创建存储过程或函数基本语法:

```sql
CREATE
    [DEFINER = user]
    PROCEDURE sp_name ([proc_parameter[,...]])
    [characteristic ...] routine_body

CREATE
    [DEFINER = user]
    FUNCTION sp_name ([func_parameter[,...]])
    RETURNS type
    [characteristic ...] routine_body

proc_parameter:
    [ IN | OUT | INOUT ] param_name type

func_parameter:
    param_name type

type:
    Any valid MySQL data type

characteristic:
    COMMENT 'string'
  | LANGUAGE SQL
  | [NOT] DETERMINISTIC
  | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
  | SQL SECURITY { DEFINER | INVOKER }

routine_body:
    Valid SQL routine statement
    
```

## [characteristic ...] 特征值部分
- LANGUAGE SQL:说明下面过程的BODY是使用SQL语言编写的，是系统默认的，为将来增加新特性做的铺垫。
- [NOT]DETERMINISTIC(确定的):代表每次的结果是否是一样的，默认是NOT DETERMINISTIC。还没有实际作用~
- 这些也都是建议，但是还没有实际作用：
  - CONTAINS SQL:表示子程序没有读写语句。默认。
  - NOT SQL:表示子程序不包含SQL语句。
  - READS SQL DATA:表示包含读数据的语句，但不包含写数据的语句。
  - MODIFIES SQL DATA:表示子程序包含写数据的语句。
- 权限问题：
  - SQL SECURITY {DEFINER | INVOKER}:可以用来指定子程序是用创建这的权限还是调用者的权限进行调用。
- COMMENT 'string'：注释

# 示例
## 无参的procedure：(单条sql的话不用改变终止符，且不用使用BEGIN、END)
```shell
mysql> create procedure test()
    -> select current_date();
Query OK, 0 rows affected (0.08 sec)

mysql> call test
    -> ;
+----------------+
| current_date() |
+----------------+
| 2019-03-25     |
+----------------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)
```
## 带IN参数的procedure：
```shell
mysql> create procedure proc_selectbyid(IN id INT)
    -> select * from actor where actor_id = id;
Query OK, 0 rows affected (0.09 sec)

mysql> call proc_selectbyid(5);
+----------+------------+--------------+---------------------+
| actor_id | first_name | last_name    | last_update         |
+----------+------------+--------------+---------------------+
|        5 | JOHNNY     | LOLLOBRIGIDA | 2006-02-15 04:34:33 |
+----------+------------+--------------+---------------------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)
```

## 带OUT参数的procedure(INOUT 类似):
```shell
mysql> create procedure test2(OUT count INT) select count(*) INTO count from actor;
mysql> call test2(@xx);
Query OK, 1 row affected (0.00 sec)

mysql> select @xx;
+------+
| @xx  |
+------+
|  200 |
+------+
1 row in set (0.00 sec)
```

## 创建函数
```shell
mysql> create function fun_sum(v1 INT,v2 INT) returns INT return (v1 + v2);
Query OK, 0 rows affected (0.09 sec)

mysql> select fun_sum(1,2);
+--------------+
| fun_sum(1,2) |
+--------------+
|            3 |
+--------------+
1 row in set (0.00 sec)
```
