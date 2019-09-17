# Mysql

一款用于存储数据且带有一定安全性的开源软件

## DBMS 数据库管理系统

Data Base Manager System.



### DDL  数据定义语言

全称：data definition language 

主要包含：CREATE、DROP、ALTER

> 使用方式

```mysql
CREATE TABLE 表名称(字段 数据类型 [注释])[引擎][字符集][注释];
DROP TABLE 表名称;
ALTER TABLE 表名 modify 字段名 字段类型 [注释];
```

以上仅为举例，其他方式遵循常用sql命令。

> 入门案例 一[不一定与以上有关]

```mysql
-- 创建数据库
CREATE DATABASE MyDataBase;
-- 若不存在则创建数据库
CREATE DATABASE IF NOT EXISTS MyDataBase;
-- 查询所有数据库
SHOW DATABASES;
-- 查询指定数据库详细信息
SHOW CREATE DATABASE MyDataBase;
-- 使用数据库
USE MyDataBase;
-- 删除数据库
DROP DATABASE MyDataBase; 
```

> 入门案例 二[不一定与以上有关]

```mysql
-- 创建表
CREATE TABLE user_1(
	userId int primary key,
    userName varchar(20) NOT NULL,
    userAge varchar(2) NOT NULL
)character set="GBK" COMMENT = "测试表"
```



### DML 数据操作语言







### DQL 数据查询语言







### DCL 数据控制语言







## 拷贝表中的数据

将另一个表中的数据拷贝到当前表

> 第一种方式 【将已存在表中的数据插入到已存在表中】

```mysql
-- 注意表中类型是否对应
insert into 备份表 select * from 被备份的表;
-- 若只需要部分数据可以这样
insert into 备份表(id,name) select id,name from 被备份的表;
```

> 第二种方式【创建临时表并插入到已存在表中】

```mysql
insert into 备份表 select 
1,"语文",60,1 UNION select
2,"数学",60,2 UNION select
3,"英语",50,3 UNION select
4,"体育",30,4 UNION select
5,"快乐",2400,1;
-- 以上为创建临时表并将临时表插入到备份表中
-- 若不需要临时表则有更好的方式替代
insert into 备份表 values
(1,"语文",60,1),
(1,"语文",60,1),
(1,"语文",60,1),
(1,"语文",60,1),
(1,"语文",60,1);
```

> 第三种方式【创表过程中将已有表中的数据插入到新表中】

```mysql
create table 表名 (user int,userName varchar(20)) select * from 被备份的表;
```

```mysql
create table 表名 select * from 被备份的表;
```



大小写不区分，但建议大写，我这里就随便写一下







## 删除数据

1.delete

```mysql
delete from 表名 [where 条件]
```

不写条件则删除全部数据

【不清空自动增长】

【可以删除被外键引用的主表字段】

2.truncate

```mysql
truncate table 表名;
```

只能删除全部数据

【清空自动增长】

【不可以删除被外键引用的主表字段】

【效率比delete高】

【delete删除数据会显示受影响的行数，truncate不会 】

















## 引擎

数据库中有许多引擎，较常用的有俩种，MyISAM InnoDB，mysql默认引擎为**innoDB.**

| 名称       | MyISAM                                       | InnoDB                                         |
| ---------- | -------------------------------------------- | ---------------------------------------------- |
| 事务处理   | 不支持                                       | 支持                                           |
| 行表锁     | 表锁，操作一行也会锁定整张表，不适合高并发。 | 行锁，操作时只锁定一行，适合高并发。           |
| 外键约束   | 不支持                                       | 支持                                           |
| 全文索引   | 支持                                         | 不支持                                         |
| 表空间大小 | 较小                                         | 较大,约2倍                                     |
| 缓存       | 只支持缓存索引，不缓存真实数据               | 不仅缓存索引，还缓存真实数据，对内存要求较高。 |
| 使用场合   | 对性能要求较高                               | 事务处理                                       |

使用不同的引擎产生保存数据的文件不同.

• InnoDB类型数据表只有一个*.frm文件，以及上一级目录的 ibdata1 文件

• MyISAM类型数据表对应三个文件：

   *.frm  --  表结构定义文件

   *.MYD  --  数据文件

   *.MYI  --  索引文件

```txt
https://segmentfault.com/a/1190000019827064?utm_source=tag-newest
```









## 约束

对数据的一些规定.

### PrimaryKey  主键约束

一个表中只能存在一个主键，

且该字段值必须唯一，

数据库为主键创建对应的索引，

主键字段可以是单字段或者是多字段的组合，

主键自动添加not null约束，

若使用自动增长必须搭配主键约束使用

> 单列

```mysql
CREATE TABLE MyTestBase(
	userId INT PRIMARY KEY AUTO_INCREMENT
);
```

> 多列组成主键

```mysql
CREATE TABLE MyTestBase(
	userId INT,
    userName varchar(20),
    Primary KEY(userId,userName)
);
```



### Not Null 非空约束

一个字段默认是null，就是说插入可以不给该字段赋值，而若添加非空约束，则插入时必须给值.

```mysql
CREATE TABLE MyTestBase(
	userId INT NOT NULL
);
```



### Unique Key 唯一约束

不出现重复值

允许出现多个NULL

可建多个唯一约束

可由多列组合而成

建唯一约束时数据库会为之建立对应的索引

如果不给唯一约束起名，该唯一约束默认与列名相同

> 单列

```mysql
CREATE TABLE MyTestBase(
	userId INT UNIQUE KEY
);
```

> 多列组成唯一

```mysql
CREATE TABLE MyTestBase(
	userId INT,
    userName varchar(20),
    UNIQUE KEY(userId,userName)
);
```



### Foreign Key 外键约束

用于建立表与表的关系.  连接。

创建外键约束后，会对多表的数据产生一定的影响。Mysql提供以下几种参照操作。

cascade：从主表删除或者更新且自动删除或更新从表中的内容。

set null：从主表中删除或者更新且从表中匹配的内容设置为null，

使用此功能必须保证从表的字段没有被设置为not null。

restrict：拒绝对父表的删除或更新操作。

使用以上三种需要使用ON DELETE拼接

No action：标准sql关键字，在mysql中与restrict相同。

> 默认为restrict

```mysql
CONSTRAINT classId FOREIGN KEY(classId) REFERENCES classBase(classid) ON DELETE restrict;
```



案例：现在 有一个 班级表 学生表，怎么让学生表中的一个学生知道自己是哪一个班级的？

解决方案：在学生表内添加一条字段，为班级的id名称，这样我就知道这个学生是属于哪一个班级了.

```mysql
CREATE TABLE ClassDB(
	classId INT PRIMARY KEY AUTO_INCREMENT
);

CREATE TABLE StudentDB(
	studentId INT PRIMARY KEY AUTO_INCREMENT 
    classId INT,
    CONSTRAINT FOREIGN KEY(classId) REFERENCES ClassDB(classId)
);
```

以上想表示的就是这个学生是属于班级的

> 格式

```mysql
 CONSTRAINT [约束名] FOREIGN KEY(从表字段名) REFERENCES 父表名(父表主键名);
```

> 测试案例

```mysql
-- 创建班级表
CREATE TABLE classBASE(
	classId INT PRIMARY KEY AUTO_INCREMENT,
	className VARCHAR(20) NOT NULL,
	classDesc VARCHAR(20)
)CHARACTER SET="utf8"

-- 创建学生表
CREATE TABLE studentBASE(
	studentId INT PRIMARY KEY AUTO_INCREMENT,
	studentName VARCHAR(20) NOT NULL,
	studentDesc VARCHAR(20),
	classId INT,
	CONSTRAINT FOREIGN KEY(classid) REFERENCES classBase(classId)
)
-- 向班级表添加数据
INSERT INTO classBase VALUE(1,"大六中","一个温馨又神秘的地方");
-- 删除班级表id = 1 的数据
DELETE FROM classbase WHERE classId = 1;
-- 向学生表添加数据
INSERT INTO studentBASE VALUE(1,"水牛","水中之牛",1);
-- 删除学生表中的外键
ALTER TABLE studentBASE DROP FOREIGN KEY classId
-- 插入外键并做一些额外的操作
ALTER TABLE studentBase ADD CONSTRAINT classId FOREIGN KEY(classId) REFERENCES classBase(classid) ON DELETE SET NULL;
-- 插入外键并做一些额外的操作
ALTER TABLE studentBase ADD CONSTRAINT classId FOREIGN KEY(classId) REFERENCES classBase(classid) ON DELETE CASCADE;
-- 查询学生表
SELECT * FROM studentBASE;
```















### 主键和唯一约束区别

主键在一个表中只能有一个，唯一约束可以有多个

主键不允许为null，唯一约束可以为null

主键可以作为外键，唯一约束不可以

主键产生的是聚集索引，唯一产生非聚集索引





## 分组查询

```mysql
select * from 表名 where 分组前条件 group by 字段 having 分组后条件. 
```











## 分页查询

```mysql
select * from 表名 limit 4,5;
```

**【注意事项】**

	1. 必须在语句最后
	2. limit 从哪里开始，显示几行







## 内连接、外连接、自连接

**【内连接】**

​	表中至少有一个匹配项则返回行，否则不显示

```mysql
select * from 主表 join 其他表 on 连接条件;
```

> 举例

```mysql
select * from student join result on student.studentNo = result.studentNo;
```

当学生学号与成绩中的学生学号一致时我才知道这个成绩是属于这个学生的.







**【外连接】**

左外连接.

**左表为主表，右表为从表**

​	若主表在从表中没有匹配项则用null补充，若从表没有主表的匹配项则不显示.

```mysql
select * from 左表 left join 右表 on 连接条件; 
```





右外连接.

**右表为主表，左表为从表**

​	若主表在从表中没有匹配项则用null补充，若从表没有主表的匹配项则不显示.

```mysql
select * from 左表 right join 右表 on 连接条件;
```











**【自连接】**

​	自己连自己，产生场景：存储菜单项，有子菜单，父菜单，但都存入菜单表内，子菜单有字段标识父菜单的id，但与父菜单在同一行，只清楚父菜单的id，不清楚父菜单的名称，但又无法使用条件匹配，因为在同一张表中，所以就要使用自连接，产生一张与自己一模一样的表，然后与它进行比较，找出父菜单.

> 以上案例解法

```mysql
select * from 菜单 AS b1 inner join 菜单 AS b2 on b1.父id = b2.id;
```

> 第二种

```mysql
select * from 菜单 AS b1,菜单 AS b2 where b1.父id = b2.id;
```





**【格式】**

```mysql
select * from 表名 AS b1 inner join 表名 AS b2 on 连接条件;
```

通过以下方式也能生成俩张一模一样的表

```mysql
select * from 菜单 AS b1,菜单 AS b2;
```















## 事务

一组操作，要么都成功，要么都失败.

平常使用的sql命令其实就是事务.因为mysql中事务是默认自动提交的，当遇到 ; 则隐式commit;

事务只针对连接对象，若再开另外一个连接对象，那么事务默认还是自动提交

> 举例

```mysql
-- 平常执行sql 命令内部其实是这样的
select * from MyDataBase;
-- 隐式提交
commit;
```



### 四个特性ACID

原子性、一致性、隔离性、持久性 .

原子性：表示一个事务是不可分割的，要么都执行，要么都失败 .

一致性：当一个事务执行完毕后，数据还是相同的 .

隔离性：事务在执行期间不应该受到其他事务的影响.

持久性：事务执行成功，数据持久存储在磁盘中.















### 使用格式

> 查询事务是否为自动提交

```mysql
show variables like '%commit%';
-- 若autocommit=on 则需要关闭事务提交
set autocommit = off;
```

> 开启事务

```java
start Transaction;
```

当事务开启时，所有执行的操作都不一定是真的，[将自动提交事务关闭后就是事务开启].

```mysql
# 一些sql命令
```

> 提交事务

```mysql
# 当所有sql命令都确保是完整的则可以选择提交
commit;
# 若有一个可能出现问题，则可以选择回滚
rollback;
```

若直接使用rollback的话只要有一条指令出错则全部回滚.

若不想全部回滚则可以使用回滚到指定地方

> 案例

```mysql
START TRANSACTION;        #开启一个事务
INSERT INTO account VALUES(1,‘张三’,1000);   #执行第一次插入
SAVEPOINT s1;             #添加保存点s1
INSERT INTO account VALUES(2,‘李四’,1000);   #执行第二次插入
SELECT * FROM account;    #显示事务提交之前的数据
ROLLBACK TO SAVEPOINT s1; #回滚到S1保存点
SELECT * FROM account;    #显示事务提交之后的数据
COMMIT;                   #提交事务，发现第二次结果没有插入。
```

【主要】

SAVEPOINT 保存点名称;

出现异常回滚到ROLLBACK TO SAVEPOINT 保持点名称 然后提交;























### 安全隐患

> 脏读【read uncommitted 读未提交】

```txt
当前事务读取到另外一个事务没有提交的数据.
```

> 不可重复读【read committed 读已提交】

```txt
当前事务读取到了另外一个事务提交的数据，导致读取提交前后数据不一致.
```

> 幻读【repeatable Read 可重复读】

```txt
当前事务读取不到另外一个事务提交的数据，虽然保证了数据读取的一致性，但可能并不是最新数据.
```

> 【Serializable 可串行化】

```txt
如果有一个连接的隔离级别设置为了串行化,那么谁先打开了事务,谁就有了先执行的权利,谁后打开事务,谁就只能等着,等前面的那个事务,提交或者回滚后,才能执行, 但是这种隔离级别一般比较少用。容易造成性能上的问题.效率比较低
```





> 查询隔离级别

```mysql
select @@tx_isolaction;
```

> 修改隔离级别

```mysql
set session transaction isolation level 隔离级别;
```



> 按效率划分【从高到低】

```txt
读未提交 > 读已提交 > 可重复读 > 可串行化
```

> 按拦截程度【从高到低】

```txt
可串行化 > 可重复读 > 读已提交 > 读未提交
```

> 总结

```txt
读未提交
> 引发问题：脏读.
读已提交
> 解决问题：脏读 引发问题：不可重复读.
可重复读
> 解决问题：脏读、不可重复读 引发问题：幻读.
可串行化
> 解决问题：脏读、幻读、不可重复读. 
================================================================================================
mysql 默认隔离级别：可重复读.
oracle 默认隔离级别：读已提交.
================================================================================================
```





> 更新丢失

```txt
A、B开启事务同时对学生表做操作，A修改了该学生名字，但B读取不到，因为是可重复读，B又修改了金额，A先提交，B后提交，此时就产生了俩种可能
1. B事务如果提交,那么A会造成修改的姓名没有了。
2. B事务回滚,那么也会造成A事务更新没有了, 因为B事务记得自己最初拿出来的数据是lisi 1000
```



> 解决方案

```mysql
# 悲观锁 for update
select * from myDataBase for update;
# 乐观锁 程序员手动写代码 可以使用同步 或 版本号
```











### 事务小案例

#### Mysql转账操作

```mysql
DELIMITER $$      #通过存储过程来实现事务封装
CREATE  PROCEDURE `pro_zhuanzhang`()  
BEGIN
	 DECLARE EXIT HANDLER FOR SQLEXCEPTION  #异常捕获 
	 BEGIN  
	     ROLLBACK;    #发生异常后回滚操作
	     SELECT '出错了...' msg;  
	 END;
 START TRANSACTION;   #开启一个事务
 UPDATE account SET money = money - 500 WHERE `name` = '张三';
 UPDATE account SET money = money + 500 WHERE `name` = '李四';
 COMMIT;    #提交事务
END$$
DELIMITER ;
```

【步骤】

 	1. 首先将结束分隔符替换成其他符号 【DELIMITER $$】
		2. 创建存储过程【CREATE  PROCEDURE 存储过程名() 】，从【BEGIN】开始，到【END】结束
		3. 声明全局异常【DECLARE EXIT HANDLER FOR SQLEXCEPTION】【BEGIN ~ END】
     		1. 只要该存储过程中出现异常，都执行该异常处理.
	4. 存储过程中开启事务
    	1. 执行需要转账的操作，若没有出现异常则提交事务，否则交给全局异常处理
	5. 将结束分隔符改为原来的 ;







#### JDBC转账操作

> 代码

```java
public class Main {

    private static BasicDataSource source = new BasicDataSource();
    private static Scanner input = new Scanner(System.in);
    static {
        source.setDriverClassName("com.mysql.jdbc.Driver");
        source.setUrl("jdbc:mysql://127.0.0.1:3306/testBase");
        source.setUsername("root");
        source.setPassword("123");
    }
    public static void main(String[] args) throws SQLException {
        Connection connection = source.getConnection();
        connection.setAutoCommit(false); //关闭事务自动提交
        user_1( connection);
    }
    public static void user_1(Connection connection){
        System.out.println("请输入需要转账多少钱：");
        int i = input.nextInt();
        try {
            //用户1
            String sql_1 = "update user_1 set userMony = userMony - ? where userName = ?";
            PreparedStatement statement = connection.prepareStatement(sql_1);
            statement.setInt(1,i);
            statement.setString(2,"张三");
		   //用户2
            String sql_2 = "update user_1 set userMony = userMony + ? where userName = ?";
            PreparedStatement statement1 = connection.prepareStatement(sql_2);
            statement1.setInt(1,i);
            statement1.setString(2,"李四");
            statement.execute();
            statement1.execute();
            connection.commit(); //若以上代码没有抛出异常则提交事务
        } catch (SQLException e) {
            try {
                connection.rollback(); //若抛出异常则回滚
                System.out.println("程序错误开始回滚");
                return;
            } catch (SQLException e1) {
                System.out.println("回滚失败"+e1);
            }
        }
        System.out.println("转账成功！");
    }
}
```

> 核心代码片段

```java
connection.setAutoCommit(false); //关闭事务自动提交
connection.commit(); //若代码没有抛出异常则提交事务
connection.rollback(); //若抛出异常则回滚
```

> cmd 回滚操作

```mysql
-- 关闭事务自动提交，进入幻境模式【自创哈哈哈哈】
set autoCommit = off;  
-- 执行操作
update MyDataBase set userMoney = UserMoney - 100 where userName = "张三";
update MyDataBase set userMoney = UserMoney + 100 where userName = "李四";
-- 若以上代码有一行执行失败则事务执行回滚.
rollback;
-- 否则提交事务
commit;
```















## 函数

\> 主要核心 方法名、返回类型、返回值 .

> 创建函数格式

```mysql
create function 函数名() returns 返回值类型
begin
	return 返回值;
end
```

> 删除函数

```mysql
drop function 函数名;
```

> 无参函数格式

```mysql
DELIMITER $$	-- 切换结束符 因为函数中可能有语句包含;所以会导致创建结束
CREATE FUNCTION `fun_noArgs`() 	-- 参数名称
RETURNS INT	-- 返回类型
BEGIN
	RETURN 1;	-- 返回值
END $$
DELIMITER ;	-- 切换回来
```

> 有参函数格式

```mysql
DELIMITER $$
CREATE FUNCTION `fun_Args`(a INT,b INT) 
RETURNS INT
BEGIN
	RETURN a + b;
END $$
DELIMITER ;
```







### 函数中定义变量

> 创建变量

```mysql
declare 变量名 数据类型 [默认值];
```

> 修改变量值

```mysql
set 变量名 = 值;
```

**【案例】**

```mysql
DELIMITER $$
create function fun_test(a int,b int) returns int
begin
	declare num1 int default a;	-- 传值也能当默认值
	declare num2 int default b;
	return num1 + num2;
end$$
DELIMITER ;
```



### 语句

#### IF语法

每个if都需要结束.

```mysql
if 条件 then 语句[条件为真则执行];
end if; -- 结束if语句
```

```mysql
if 条件 then 语句;
elseif 条件 then 语句;
end if;
```

```mysql
if 条件 then 语句;
elseif 条件 then 语句;
else 语句;
end if;
```

**【案例演示】**

```mysql
delimiter $$
create function anl_001(val int) returns varchar(20)
begin 
	if val = 1 then return "牛批";
     elseif val = 2 then return "优秀";
     else return "傻逼";
     end if;	
end$$
delimiter;
```













#### CASE语法

条件区间判断

> switch用法 case后加值

等值判断

```mysql
delimiter $$
create function swt_001(`select` int) returns varchar(20)
begin 
    case `select`
        when 1 then return "星期一";
        when 2 then return "星期二";
        when 3 then return "星期三";
        when 4 then return "星期四";
        else return "没有了";
    end case;
end$$
delimiter;
```

> if用法 case后不能加值

区间判断

```mysql
delimiter $$
create function if_001(`course` int) returns varchar(20)
begin 
    case
        when course >= 90 then return "真棒";
        when course >= 80 then return "牛批";
        when course >= 70 then return "666";
        when course >= 60 then return "嘟嘟嘟";
        else return "没有了";
    end case;
end$$
delimiter;
```











#### loop 循环

类似while循环默认为true，死循环 

跳出循环的方式有：return、LEAVE 循环名称、ITERATE 循环名称

```mysql
delimiter $$
create function loop_001() returns int
begin
	[循环名称:] loop	-- :是必须的
		-- 循环中最低有一条语句
		return 0;
	end loop [循环名称];
end$$
delimiter ;
```

> LEAVE 循环名称	java中的break	跳出循环

循环与LEAVE都必须加上循环名称，否则报错，end loop 建议加上否则嵌套循环会混乱.

```mysql
delimiter $$
create function loop_001() returns int
begin
	ccc: loop
		LEAVE ccc;
	end loop ccc;
end$$
delimiter ;
```

> ITERATE 循环名称		java中的continue		跳出本次循环，执行下次循环

循环与ITERATE 都必须加上循环名称，否则报错，end loop 建议加上否则嵌套循环会混乱.

```mysql
delimiter $$
create function loop_001() returns int
begin
	ccc: loop
		ITERATE ccc;
	end loop ccc;
end$$
delimiter ;
```







#### REPEAT 循环

类似java中的for循环   当 UNTIL中的条件满足则跳出循环

> 语法格式

```mysql
REPEAT
	SET a = a+1;
	UNTIL a > 100 -- 不需要加;号
END REPEAT;
```

**【案例演示】**

从1加到101

```mysql
DELIMITER $$
CREATE FUNCTION test_001() RETURNS INT
BEGIN
	DECLARE a,b INT DEFAULT 0;
	REPEAT
		SET a = a+1;
		UNTIL a > 100	
	END REPEAT;
	RETURN a;
END$$
DELIMITER ;
```



















## 存储过程

类似于java的方法

```mysql
Delimiter $$
create procedure fun_1()
begin
end$$
Delimiter ;
```















## 索引

常见索引：

​	主键、唯一、常规，全文索引.



**常规索引.**

> Index 创建三种方式

```mysql
create index 索引名 table 表名(字段名);
```

```mysql
alter table 表名 add 索引名 index 表名(字段名);
```

```mysql
CREATE TABLE  `result` (
#省略一些代码
INDEX/KEY `ind` (`studentNo`, `subjectNo`)  -- key || index 都为常规索引
)
```

> 删除常规索引

```mysql
DROP INDEX 索引名 ON 表名
```

```mysql
ALTER TABLE 表名 DROP INDEX 索引名
```



**全文索引.**

​	\> 作用	

​		快速定位特定数据

​	\> 注意

​		只能用于MyISAM类型的数据表

​		只能用于 CHAR 、 VARCHAR、TEXT数据列类型

​		适合大型数据集

> 建立全文索引

```mysql
CREATE TABLE `student` (
#省略一些SQL语句
	FULLTEXT (`StudentName`); 
)ENGINE=MYISAM;
```

> 追加索引

```mysql
alter table 表名 add FULLTEXT(`StudentName`);
```

> 删除索引

```mysql
alter table 表名 drop FULLTEXT(`StudentName`);
```





索引的作用？

​	1.增加查询效率

哪些列适合创建索引？

​	1.经常查询的、不经常改动、字段内容少

使用索引的注意事项？

   	EXPLAIN可以帮助开发人员分析SQL问题,

​	EXPLAIN显示了mysql如何使用索引来处理select语句以及连接表,

​	可以帮助选择更好的索引和写出更优化的查询语句。



索引虽然好处很多，但过多的使用索引可能带来相反的问题，索引也是有缺点的：

​	虽然索引大大提高了查询速度，同时却会降低更新表的速度，如对表进行INSERT,UPDATE和DELETE。因为更新表时，mysql不仅要保存数据，还要保存一下索引文件

​	建立索引会占用磁盘空间的索引文件。一般情况这个问题不太严重，但如果你在要给大表上建了多种组合索引，索引文件会膨胀很宽

​      索引只是提高效率的一个方式，如果mysql有大数据量的表，就要花时间研究建立最优的索引，或优化查询语句。

​     使用索引时，有一些技巧：

​    1.索引不会包含有NULL的列

​       只要列中包含有NULL值，都将不会被包含在索引中，复合索引中只要有一列含有NULL值，那么这一列对于此符合索引就是无效的。

​    2.使用短索引

​       对串列进行索引，如果可以就应该指定一个前缀长度。例如，如果有一个char（255）的列，如果在前10个或20个字符内，多数值是唯一的，那么就不要对整个列进行索引。短索引不仅可以提高查询速度而且可以节省磁盘空间和I/O操作。

​    3.索引列排序

​       mysql查询只使用一个索引，因此如果where子句中已经使用了索引的话，那么order by中的列是不会使用索引的。因此数据库默认排序可以符合要求的情况下不要使用排序操作，尽量不要包含多个列的排序，如果需要最好给这些列建复合索引。

​    4.like语句操作

​      一般情况下不鼓励使用like操作，如果非使用不可，注意正确的使用方式。like ‘%aaa%’不会使用索引，而like ‘aaa%’可以使用索引。

​    5.不要在列上进行运算

​    6.不使用NOT IN 、<>、！=操作，但<,<=，=，>,>=,BETWEEN,IN是可以用到索引的

​    7.索引要建立在经常进行select操作的字段上。

​       这是因为，如果这些列很少用到，那么有无索引并不能明显改变查询速度。相反，由于增加了索引，反而降低了系统的维护速度和增大了空间需求。

​    8.索引要建立在值比较唯一的字段上。

​    9.对于那些定义为text、image和bit数据类型的列不应该增加索引。因为这些列的数据量要么相当大，要么取值很少。

​    10.在where和join中出现的列需要建立索引。

​    11.where的查询条件里有不等号(where column != …),mysql将无法使用索引。

​    12.如果where字句的查询条件里使用了函数(如：where DAY(column)=…),mysql将无法使用索引。

​    13.在join操作中(需要从多个数据表提取数据时)，mysql只有在**主键和外键的数据类型相同**时才能使用索引，否则及时建立了索引也不会使用。







## 视图

**【概念】**

​	视图是一张虚拟表，视图中不存放数据，存放查询sql的命令.

![view](images\view.png)



> 创建视图

```mysql
create view view_test01 
as 
select sName,sAge from student;
```

> 删除视图

```mysql
drop view 视图名;
```

> 调用视图

```mysql
select * from 视图名;
```



**【作用】**

​	将复杂的sql命令一次封装，终身使用.



**【注意事项】**

​	视图中可以使用多个表

​	一个视图可以嵌套另一个视图 

​	对视图数据进行添加、更新和删除操作直接影响所引用表中的数据

​	当视图数据来自**多个表**时，不允许添加和删除数据



























## 常用sql命令

```mysql
创建数据库
CREATE DATABASE 数据库名;
删除数据库
DROP DATABASE 数据库名;
查询数据库
SHOW DATABASES;
使用数据库
USE 数据库名;
=========================
查询表
SHOW TABLES;
创建表
CREATE TABLE 表名(
     字段名 数据类型 [列属性][注释]
);
删除表
DROP TABLE 表名;
修改表
ALTER TABLE 表名 RENAME AS 新表名;
=========================
字段添加
ALTER Table 表名 ADD 字段名 数据类型 [列属性][注释];
字段删除
ALTER Table 表名 DROP 字段名;
字段修改
ALTER Table 表名 MODIFY 字段名 数据类型 [列属性][注释]; --> 不能修改字段名
ALTER Table 表名 CHANGE 字段名 新字段名 数据类型 [列属性][注释];--> 能修改字段名
字段查询
SELECT 字段名 FROM 表名;
=========================
数据添加
INSERT INTO 表名[参数] VALUES(表内字段);
INSERT INTO 表名[参数] VALUES(表内字段),(表内字段),(表内字段);
数据修改
UPDATE 表名 SET [字段1 = 值] [字段2 = 值] where [条件筛选]
数据删除
DELETE FROM 表名 WHERE 字段 = 数据;
清除表内所有数据
truncate table testBase;
数据查询
SELECT * FROM 表名;
==========================
约束：
	PRIMARY KEY 主键
	UNIQUE KEY 唯一键
	FOREIGN KEY 外键   --> 使用  FOREIGN KEY(字段名) REFERENCES 表名(字段名);
==========================
外键约束创建会对会对多表的数据产生一定的影响。Mysql提供以下几种参照操作
cascade：从主表删除或者更新且自动删除或更新从表中的内容。
set null：从主表中删除或者更新且从表中匹配的内容设置为null，使用此功能必须保证从表的字段没有被设置为not null。
restrict：拒绝对父表的删除或更新操作。
No action：标准sql关键字，在mysql中与restrict相同。
=====================================
添加主键约束：
ALTER TABLE 表名 add constraint 约束名 PRIMARY KEY 表名(主键字段);
删除主键约束：
ALTER TABLE 表名 DROP PRIMARY KEY;
添加外键约束：
alter   table 从表   add  constraint  约束名 foreign   key  (从表外键字段)   references  主表  （主键字段）;
删除外键约束：
ALTER TABLE 表名 DROP FOREIGN KEY 外键约束名;


set names 编码;
```







## 自定义连接池

功能：实现连接的重复使用.

实现DataSource接口，重写全部方法，具体实现调用JDBC包，实现类内部使用集合存储con连接，在初始化的时候提前向集合中添加多个连接对象，每当有用户调用getConnaction则删除集合中第一个元素，并将这个元素返回出去给用户使用，再将这个元素通过装饰者模式通过构造方法加入给装饰者类【目的：当使用Conn调用close不执行关闭操作，执行归还】，当集合中若没有连接对象则执行扩容操作，向集合中for循环再次添加x个，当用户调用close时执行装饰者的close将先前的连接对象归还.

> 部分代码片段

```java
class MyDataSource implements DataSource{
    private List<Connection> list = new ArrayList();
    public MyDataSource(){
        for(int i=0;i<10;i++){
            list.add(DriverManager.getConnection());
        }
    }
    @Override
    public Connection getConnection(){
        Conncetion oldConn = list.remove(0);
        MyClose myclose = new MyClose(oldConn,list);
        return myclose;
    }
}
```

> 装饰者

```java
class MyClose implements Connection{
    private Connection conn = null;
    private List<Connection> list = null;
    public MyClose(Connection conn,List<Connection> list){
        this.conn = conn;
        this.list = list;
    }
    
    public void close(){
        list.add(conn);
    }
}
```
