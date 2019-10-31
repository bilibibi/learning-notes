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

