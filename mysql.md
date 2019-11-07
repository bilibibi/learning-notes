source: <https://www.bilibili.com/video/av49181542>

#### 基础命令

```mysql
DESC seckill;

# 字符函数
# length 获取参数值的字节个数
select LENGTH('fangyj');
select LENGTH('方余佳');

SHOW VARIABLES LIKE '%char%';

# 拼接字符串
select CONCAT('hello',' ','world');

# substr、substring
# 截取从指定索引处后面所有字符
select SUBSTR('testsubstr',5);
# 截取从指定索引处指定字符长度的字符
select SUBSTR('substrtest',1,6);

# instr 返回字符串第一次出现的索引，找不到返回0
select INSTR('testinstr','instr');

# trim
select TRIM('test' from 'testtrimtest');

# lpad 用指定字符实现左填充指定长度
select LPAD('test',10,'lpad');

# rpad 用指定的字符实现右填充指定长度
select RPAD('test',10,'rpad');

# replace 替换
select REPLACE('testreplacetest','test','lala');


# 数学函数
# round 四舍五入
select ROUND(1.123,2);

# ceil 向上取整，返回>=该参数的最小整数
select CEIL(1.1);

# floor 向下取整，返回<=该参数的最小整数
select FLOOR(1.9);

# truncate 截断
select TRUNCATE(1.666,1);

# mod 取余
select MOD(10,3);

# 日期函数
# now 返回当前系统日期+时间
select NOW();

# curdate 返回当前系统日期，不包含时间
select CURDATE();

# curtime 返回当前时间，不包含日期
select CURTIME();

# 获取指定的部分，年、月、日、小时、分钟、秒
select YEAR(NOW()), MONTH(NOW()), DAY(NOW());

# str_to_date 将字符通过指定格式转换成日期
select STR_TO_DATE('1991-11-26 12:11:12','%Y-%m-%d %H:%i:%s');

# date_format 将日期转换成字符
select DATE_FORMAT(NOW(),'%Y/%m/%d %H%i%s');


# 流程控制函数
# if函数
select IF(10<5,'大','小');

# case函数
select 
case 10
when 5 then 'small'
when 10 then 'equal'
else 'no found'
end;

select
case
when 5>1 then 'when1'
when 5>5 then 'when2'
else 'else'
end;

###################

# 分组函数
# sum 求和
# avg 平均值
# max 最大值
# min 最小值
# count 计算个数

###################

CREATE TABLE test(
	id INT(64),
	name VARCHAR(20),
	sex TINYINT(1)
);

# 修改表
DESC test;
ALTER TABLE test CHANGE COLUMN `name` title VARCHAR(20);
ALTER TABLE test MODIFY COLUMN title VARCHAR(25);
ALTER TABLE test ADD COLUMN `name` VARCHAR(20);
ALTER TABLE test DROP COLUMN `name`;
ALTER TABLE test RENAME TO test_sql;

# 删除表
DROP TABLE IF EXISTS test;

# 复制表
# 仅复制表结构
CREATE TABLE new_table LIKE old_table;
# 复制表结构+数据
CREATE TABLE new_table
SELECT * FROM old_table;

###################

# 查看隔离级别
SELECT @@tx_isolation;
# 设置隔离级别
# SET SESSION|GLOBAL TRANSACTION ISOLATION LEVEL 隔离级别；

###################

# 创建视图
CREATE VIEW view_name AS SELECT ...;

# 修改视图
CREATE OR REPLACE AS view_name AS SELECT ...;
ALTER VIEW view_name AS SELECT ...;

# 删除视图
DROP VIEW view_name, view_name2,...;

# 查看视图
DESC view_name;
SHOW CREATE VIEW view_name;

###################

# 查看所有的会话变量
SHOW VARIABLES;
# 查看所有的全局变量
SHOW GLOBAL VARIABLES;
# 查看部分的全局变量
SHOW GLOBAL VARIABLES LIKE '%char%';

# 查看指定的会话变量
SELECT @@autocommit;
# 查看指定的全局变量
SELECT @@GLOBAL.autocommit;

# 为某个指定的会话变量赋值
SET autocommit=0;
SET SESSION autocommit=1;
SET @@autocommit=1;
# 为某个指定的全局变量赋值
SET @@GLOBAL.autocommit=1;

# 设置用户变量
SET @fangyj='king';
SET @fangyj:='king';

###################

# 存储过程：可以有0个或多个返回，适合做批量插入、批量更新
# 创建
DELIMITER $ # 定义存储过程结束标识
CREATE PROCEDURE 存储过程名(IN|OUT|INOUT paramname VARCHAR(10))
BEGIN
	...
END $

# 调用
CALL 存储过程名(实参列表)$

# 查看存储过程的信息
SHOW CREATE PROCEDURE 存储过程名;

# 删除
#DROP PROCEDURE 存储过程名;

###################

# 函数：有且仅有1个返回，适合做处理数据返回一个结果
# 创建
CREATE FUNCTION 函数名(参数列表) RETURNS 返回类型
BEGIN
	函数体
END

# 调用
SELECT 函数名(参数列表)

# 查看函数
SHOW CREATE FUNCTION 函数名;

# 删除
#DROP FUNCTION 函数名;

# eg:
CREATE FUNCTION fun1() RETURNS INT
BEGIN
	DECLARE total INT DEFAULT 0; # 定义局部变量
	SELECT COUNT(*) INTO total from test;
	return total;
END
SELECT fun1();

CREATE FUNCTION fun2(tid INT) RETURNS VARCHAR(10)
BEGIN
	SET @tname=''; # 定义用户变量
	SELECT `name` INTO @tname from test where id = tid;
	RETURN @tname;
END
SELECT fun2(1);

###################

# 流程控制结构
# 分支控制
/*
IF(expr1,expr2,expr3)

CASE case_value
	WHEN when_value THEN
		statement_list
	ELSE
		statement_list
END CASE;

CASE
	WHEN when_value THEN
		statement_sql
	ELSE
		statement_sql
END CASE;

# 次方式必须放在 BEGIN...END 中
IF search_condition THEN
	statement_list
ELSEIF search_condition THEN
	statement_list
ELSE
	statement_list
END IF;
*/

# 循环结构
/*
# 都需要放到 BEGIN...END 中
1. WHILE
[label:] WHILE search_condition DO
	statement_list
END WHILE [label];

2. LOOP
label: LOOP
	statement_list

	IF exit_condition THEN
		LEAVE label; 
	END IF; 
END LOOP label;

3. REPEAT
[label:] REPEAT
	statement_list
UNTIL search_condition 
END REPEAT [label];
*/
```



#### 高级特性

##### 机读顺序

1. FROM
2. ON
3. JOIN
4. WHERE
5. GROUP BY
6. HAVING
7. SELECT
8. DISTINCT
9. ORDER BY
10. LIMIT



##### Explain

1. **id**：执行顺序

   1. id相同，执行顺序由上到下
   2. id不同，值越大，优先级越高，越先执行

2. select_type：查询类型，用于区别普通查询、联合查询、子查询等复杂查询

   1. SIMPLE：简单查询，不包含子查询或UNION
   2. PRIMARY：查询中若包含任何复杂的子部分，最外层查询则被标记为primary
   3. SUBQUERY：在SELECT或WHERE列表中包含子查询
   4. DERIVED：在FROM列表中包含的子查询被标记为DERIVED(衍生)
   5. UNION：若第二个SELECT出现在UNION之后，则被标记为UNION；
      若UNION包含在FROM子句的子查询中，外层SELECT将被标记为DERIVED
   6. UNION RESULT：从UNION表获取结果的SELECT

3. table：表名

4. **type**：查询类型 system > const > eq_ref > ref > range > index > ALL

5. possible_keys：可能应用到的索引

6. **key**：实际使用的索引

7. key_len：索引中使用的字节数，可通过该列计算查询中使用的索引长度，在不损失精确性的情况下，长度越短越好

8. ref：显示索引的那一列被使用了，如果可能的话，是一个常数。

9. **rows**：大致估算出找到所需的记录所需要读取的行数

10. **Extra**：包含不适合在其他列中显示但十分重要的信息

    1. **Using filesort**：文件排序，无法利用索引完成排序

    2. **Using temporary**：使用了临时表保存中间结果，在对查询结果排序时使用临时表，常见于排序和分组查询

    3. **Using index**：表示相应的select操作中使用了覆盖索引(Covering Index)，避免访问了表的数据行
       如果同时出现Using where，表明索引被用来执行索引键值的查找；
       如果没有同时出现Using where，表明索引用来读取数据而非执行查找动作。

       > 覆盖索引(Covering Index)，也叫索引覆盖
       >
       > 查询的数据只用从索引中就能够取得，不必读取数据行，即查询列被所建的索引覆盖。

    4. Using where：表示使用了where过滤

    5. Using join buffer：使用了连接缓存

    6. Impossible where：where子句的值总是false，不能获取到任何元组

    7. select tables optimized away：在没有GROUPBY子君的情况下，基于索引优化MIN/MAX操作或者对于MyISAM存储引擎优化COUNT(*)操作，不必等到执行阶段再进行计算，查询执行计划生成的阶段即完成优化。

    8. distinct：优化distinct操作，在找到第一匹配的元组后即停止找同样值得动作



##### 索引

**索引优化**

1. 尽可能减少join语句中的NestedLoop的循环总次数；

2. 永远用小结果集驱动大的结果集；

3. 优先优化NestedLoop的内层循环；

4. 保证join语句中被驱动表上join条件字段已经被索引；

5. 当无法保证被驱动表的join条件字段被索引且内层资源充足的前提下，不要太吝啬JoinBuffer的设置；

**索引失效**

1. 最左前缀法则；
2. 不在索引列上做任何操作(计算、函数、(自动或手动)类型转换)，会导致索引失效；
3. 存储引擎不能使用索引中范围条件右边的列；
4. 尽量使用覆盖索引(只访问索引的查询)，减少select *；
5. 使用不等于(!= 或 <>)时无法使用索引；
6. 使用is null 或 is not null 时无法使用索引；
7. like以通配符%开头时无法使用索引，可以通过覆盖索引避免全表扫描；
8. 字符串不加单引号时索引会失效；
9. 使用or时索引会失效；



##### 查询优化

1. **永远小表驱动大表**
   select * from A a where a.id in (select b.aid from B b); -> 当A表数据大于B表时用in
   select * from A a where exists (select 1 from B b where a.id = b.aid); -> 当A表数据小于B表时用exists
2. order by 关键字优化
   1. 尽量使用Index方式排序，避免使用FileSort方式
   2. order by 语句使用索引最左前缀字段
   3. 使用 where 子句与 order by 子句条件列组合满足索引最左前缀
3. group by 关键字优化
   1. 先排序后进行分组，遵照索引建的最佳左前缀
   2. 当无法使用索引列，增大 max_length_for_sort_data 参数的设置和增大 sort_buffer_size 参数的设置
   3. where 高于 having，能写在 where 限定条件里就不要去 having 限定



##### profiles

1. 查看当前mysql版本是否支持：show variables like 'profiling';
2. 开启记录功能：set profiling=on;
3. 查看结果：show profiles;
4. 诊断SQL：show profile cpu, block io for query 上一步中问题SQL数字号码;
5. 需要注意的结论：
   1. converting HEAP to MyISAM => 查询结果太大，内存
   2. Creating tmp table => 创建临时表
   3. Copying to tmp table on disk => 把内存中临时表复制到磁盘(危险)
   4. locked => 锁死





