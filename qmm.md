# 基础

SQL分类

- DDL，数据定义语言
- DML，数据操作语言
- DQL，数据查询语句
- DCL，数据控制语言

## DDL:

- **查询** 

- - SHOW DATABASES；所有的库
  - SELECT DATABASE(); 当前所在的库

- **创建**

- - CREATE DATABASE [IF NOT EXISTS] 库名 [default charset 字符集] (COLLATE 排序规则)  ----设置字符集不建议utf8，这只有三个字节，可以采用utf8mb4，占四个字节

- **删除**

- - DROP DATABASE [IF EXISTS] 库名

- **使用**

- - USE 库名

- **表操作-查询**

- - SHOW TABLES； (查询当前数据库的所有表)
  - DESC 表名；（查询表结构）
  - SHOW CREATE TABLE 表名； （查询指定表的建表语句）

- **表操作-创建**

- - ~~~sql
    CREATE TABLE 表名 (
    	字段1 字段1类型【COMMENT 字段1注释】,
    	字段2 字段2类型【COMMENT 字段2注释】
    )[COMMENT 表注释]；
    ~~~

- **表操作-数据类型**

- **表操作-修改**

- - ~~~sql
    添加字段
    ALTER TABLE 表名 ADD 表字段 类型（长度）comment 注释 【约束】;(添加字段)
    ~~~

    ~~~sql
    修改数据类型
    ALTER TABLE 表名 MODIFY 字段名 新数据类型 ；

    修改字段名和字段类型
    ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型 COMMENT 注释 【约束】；

    删除字段
    ALTER TABLE 表名 DROP 字段名；

    修改表名
    ALTER TABLE 表名 RENAME TO 新表名;
    ~~~

- **表操作-删除**

- - ~~~sql
    删除表
    DROP TABLE [IF EXISTS] 表名；

    删除指定表，并重新建表
    TRUNCATE TABLE 表名；
    ~~~

  - ​

## DML：完成数据库的增删改

- 添加数据

- - ~~~sql
    给指定的字段添加
    INSERT INTO 表名 （字段1，字段2）VALUES(值1，值2)；

    给全部字段添加数据
    INSERT INTO 表名 VALUES（值1，值2）；

    批量添加
    INSERT INTO 表名 （字段1，字段2）VALUES(值1，值2)，(值1，值2)，(值1，值2)；
    INSERT INTO 表名 VALUES（值1，值2），(值1，值2)，(值1，值2)，(值1，值2)；
    ~~~

- 修改数据

- -  ~~~sql
    UPDATE 表名 字段1=值1，字段2=值2，【where 条件】；
    ~~~

- 删除数据

- - ~~~sql
    DELETE FROM 表名 【WHERE 条件】；
    ~~~

  - ​

## DQL：查询

- 基础查询

- - ~~~sql
    SELECT 字段列表（eg.字段1，字段2.。。） FROM 表名；
    SELECT * FROM 表名；

    设置别名
    SELECT 字段1【AS 别名1】,... FROM 表名;

    去除重复记录
    SELECT DISTINCT 字段列表 FROM 表名;
    ~~~

  - **查询**

  - ~~~sql
    条件查询
    SELECT 字段列表 FROM 表名 WHERE 条件；

    聚合函数
    SELECT 聚合函数(字段列表) FROM 表名;
    ->count  max  min  avg  sum  在计算时，null值不参与计算

    分组查询
    SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件]
    ->WHERE中不能使用聚合函数，HAVING可以使用聚合函数

    排序查询
    SELECT 字段列表 FROM 表名 ORDER BY 字段1，排序方式1，字段2，排序方式2;
    ->ASC 升序；DESC 降序； 先按照字段1排序，如果值相同，则按照字段2排序。

    分页查询
    SELECT 字段列表 FROM 表名 LIMT 起始索引，查询记录数；
    ->起始索引从0开始，如果查询的是第一页，可以省略，简写为limit 10
    ~~~

  - DQL的执行顺序

  - - ~~~SQL
      FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY ->LIMIT
      ~~~


## DCL管理数据库的用户与库的访问权限：

- - ~~~sql
    查询用户
    USE mysql;
    SELECT * FROM user;

    创建用户
    CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';

    修改用户密码
    ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';
    ->IDENTIFIED WITH 指定加密方式

    删除用户
    DROP USER '用户名'@'主机名'

    必须通过用户名与主机名才能准确定位

    查询权限
    SHOW CRANTS FOR 'user'@'host';

    授予权限
    GRANT 权限列表 ON 库.表 TO 'user'@'host';

    撤销权限
    REVOKE 权限列表 ON 库.表 FROM 'user'@'host';
    ~~~

  - ​


## 函数

- 字符串函数

- - ![79300544813](C:\Users\Administrator\AppData\Local\Temp\1793005448134.png)

  - ~~~sql
    -- concat
    select CONCAT('Hello','MySQL');
    ~~~


    -- lower
    select LOWER('HELLO');
    
    
    -- upper
    SELECT UPPER('hello'); 
    
    -- lpad
    SELECT LPAD('start',10,'-');
    
    -- rpad
    SELECT RPAD('start',10,'-');
    
    -- trim
    SELECT TRIM('   Hello World   ');
    
    -- substring
    SELECT SUBSTRING('Hello MySQL',1,5);//从1开始
    
    eg.
    -- 给workno编号设置为5位，不足的向前补0
    UPDATE emp SET workno = LPAD(workno,5,'0');
    ​~~~

- 数值函数

- - ![79300642273](C:\Users\Administrator\AppData\Local\Temp\1793006422737.png)

- 日期函数

- - ![79300648490](C:\Users\Administrator\AppData\Local\Temp\1793006484908.png)

- 流程控制函数

- - | 函数                                          | 功能                                         |
    | --------------------------------------------- | -------------------------------------------- |
    | IF(value,t,f)                                 | 如果value为true，则返回t，否则返回f          |
    | IFNULL(value1,value2)                         | 如果value1不为空，返回value1，否则返回value2 |
    | CASE WHEN val1 THEN res1 ...ELSE default END  | 如果val1为true，返回res1，否则返回default    |
    | CASE expr WHEN val1 THEN res1 ELSE defult END | 如果expr值为val1，返回res1，否则返回default  |

  - ~~~sql
    -- 查询emp表的员工姓名和工作地址（北京/上海 ---->  一线城市，其他 ----> 二线城市）
    select name,
    (CASE workaddress WHEN '北京' THEN '一线城市' WHEN '上海' THEN '一线城市' ELSE '二线城市' END) AS 城市级别 FROM emp;
    ~~~


  - ​

## 约束

| 约束     | 描述                                 | 关键字      |
| -------- | ------------------------------------ | ----------- |
| 非空约束 | 限制字段不为NULL                     | NOT NULl    |
| 唯一约束 | 保证字段的所有数据都是唯一的         | UNIQUE      |
| 主键约束 | 主键是一行数据的唯一标识，非空且唯一 | PRIMARY KEY |
| 默认约束 | 保存数据时，未指定字段。。。         | default     |
| 检查约束 | 保证字段满足某一条件                 | CHECK（）   |
| 外键约束 | 让两张表的数据之间建立连接           | FOREIGN KEY |

- SQL复制

  ```
  -- 添加外键
  CREATE TABLE 表明(
  	字段名 数据类型,
    [CONSTRAINT] [外键名称] FOREIGN KEY 外键字段 REFRENCES 主表（主表列名)
  );
  ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY 外键字段 REFERENCES 主表（主表列名）;

  -- 删除外键
  ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
  ```

- 删除/更新行为

- | 行为        | 说明                                                         |
  | ----------- | ------------------------------------------------------------ |
  | NO ACTION   | 在父表中进行删除/更新，会检查是否存在外键，如果存在，则不允许操作（同RESTRICT） |
  | RESTRICT    |                                                              |
  | CASCADE     | 。。。，如果存在，则操作在子表中的记录                       |
  | SET NULL    | 。。。，先检查是否存在对应外键，存在则设置改外键中的值为NULL（所以外键得允许取NULL） |
  | SET DEFAULT | 父表变更时，子表将外键列 设置成一个默认的值（Innodb不支持）  |

  SQL复制

  ```
  -- 在建立外键时设置  eg.
  alter table employee add constraint fk_emp_dept_id foreign key (dept_id) references dept(id) on update cascade on delete cascade;
  ```

  ​

## 多表查询

- 连接查询

- - 内连接：查询A,B之间的交集

  - - ~~~sql
      -- 隐式内连接
      SELECT 字段列表 FROM 表1,2,3	WHERE 。。。;

      -- 显示内连接
      SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 连接条件...;
      eg.
      SELECT e.name,d.name FROM emp e [INNER] JOIN dept d ON e.dept_id = d.id;
      ~~~

    - ​

  - 外连接：

  - - 左外连接：查左表的所有数据，以及两张表交集部分数据

    - - ~~~sql
        SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 连接条件;
        -- 如果on中写了一个过滤条件，表中未满足过滤条件的数据不会消失，但可能导致出现部分字段为null
        -- 如age>40 and aid = id，如果age<=40,则这条数据会存在，但是对于右表的字段会显示未null
        ~~~

      - ​

    - 右外连接：查右表的所有数据，以及两张表交集部分数据

    - eg. A表与B表，交集为C表，则左连接就是获取 A+C 的数据

  - 自连接：当前表与自身的连接查询，自连接必须使用表别名

  - ~~~sql
    -- 技巧：一张表可以看成两张表，两个emp表，所以需要设计别名，不然容易混乱
    select a.name as 部下,b.name as 领导 from emp a,emp b where a.managerid = b.id;

    --查询所有员工emp及其领导，没有领导的也要查出来
    SELECT a.name as 员工,b.name as 领导 FROM emp a left join emp b on a.managerid = b.id;
    ~~~

  - 联合查询-union，union all

  - ~~~sql
    SELECT ... FROM 表A ...
    UNION [ALL]
    SELECT ... FROM 表B ...;
    -- 会将两次的查询结果合并为一个新的表
    -- union all 会查询重复的内容，如果只有union则会删去重复的内容
    ~~~

  - ​

- 子查询(嵌套子查询)

- - 标量子查询（子查询结果为单个值）
  - 列子查询（结果为一列）
  - 行子查询（结果为一行）
  - 表子查询（结果为多行多列）
  - 子查询位置可以在select 。。。  |   from 。。。| where 。。。

~~~sql
SELECT * FROM T1 WHERE COLUMN1=(SELECT column1 FROM T2);

-- 查找销售部的员工信息   标量子查询
select * from emp where dept_id = (select id from dept where name = '销售');

-- 查询方东白之后入职的员工
select * from emp where entrydate > (select entrydate from emp where name = '方东白')


-- 列子查询
-- 查找‘销售’与‘市场’的员工
select * from emp where dept_id IN(select id from dept where name IN('销售','市场'));

-- 找到工资比财务所有人工资都高的员工
select * from emp where salary > all( select salary from emp where dept_id = (select id from dept where name='财务'));

-- 找到工资比研发部门任一员工工资高的员工
select * from emp where salary > some(select salary from emp where dept_id = (select id from dept where name='研发'));

-- 行子查询
-- 查找与张无忌的薪资及直属领导相同的员工
 select * from emp where (salary,managerid) = (select salary,managerid from emp where name = '张无忌');
 
 -- 表子查询

~~~

| 操作符（用于列子查询） | 描述                               |
| ---------------------- | ---------------------------------- |
| ANY                    | 子查询列表中，有任意一个满足的即可 |
| SOME                   | 与SOME同等                         |
| ALL                    | 子查询返回列表的所有值都得满足     |

## 事务

定义：是一组操作的集合，必须全部通过才能成功，否则算作失败。这些操作只有同时成功或者同时失败。

示例表：

~~~sql
create table account (
  id int auto_increment primary key comment '主键',
  name varchar(10) comment '姓名',
  money int comment '余额'
) comment '账户表';

insert into account values(null,'张三',2000),(null,'李斯',2000);

--查看事务
select @@autocommit;   -- 如果为1，则表示默认提交，为0，表示手动提交
set @@autocommit = 0;

-- 提交事务
commit;

-- 回滚事务
rollback;

-- 或者
-- 开启
start transaction/begin;

-- 提交
commit;

-- 回滚
rollback;


-- 操作（张三给李斯转1000）
select @@autocommit; 
set @@autocommit = 0;

select * from account where name = '张三';
update account set money = 1000 where name = '张三';
cheng baocuo...
update account set money = 3000 where name = '李斯';

commit;

rollback;
~~~

**事务的四大特性**

- 原子性：事务是最小的操作单元，只有全部成功与全部失败
- 一致性：事务完成时，必须使所有数据保持一直状态
- 隔离性：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行
- 持久性：事务一旦提交或者回滚，对数据库中的数据更改是永久的

**并发事务问题**

- 脏读：一个事务读到另一个事务还没有提交的数据（可以通过设置隔离级别，来确定是否读取未提交版本或是快照版本）
  - 如事务a有update数据库，同时事务b在select，但是此时事务a还没有提交，这就是脏读
- 不可重复度：一个事务先后读取同一条记录，但两次读取的数据不同
- - 事务A区读取一条数据，但是在这之后事务B对该数据有修改，如果此时事务A再次读取就会导致结果不一致
- 一个书屋按条件查询数据时，没有对应的数据行，但是在插入数据后，又发现这行数据已存在，好像出现了幻影

**事务隔离级别**

- Read uncommited： 三个问题都会出现
- Read commited：脏读问题不会出现
- Repeattable Read（默认）：只会出现幻读问题
- Seializable：都不会出现，设置这个level的会话会占住这个锁，其他会话只能等这个会话结束

~~~sql
-- 查看事务隔离级别
SELECT @@TRANSACTION_ISOLATION;

-- 设置事务隔离级别
SET [SESSION | GLOBAL] TRANSACTION ISOLATION LEVEL [事务隔离级别]
~~~

