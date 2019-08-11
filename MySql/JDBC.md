# JDBC

一款 Java 连接数据库的jar包

JDBC API主要功能：

​	具体通过以下类/接口实现：

​		DriverManager ：管理JDBC 驱动

​		Statement（PreparedStatement）：增删查改

​		CollableStatement：调用数据库中的存储过程/存储函数

​		Result：返回的结果集

Connection 产生操作数据库的对象：

​	① Connection 产生 statement 对象：createStatement();

​	② Connection 产生 PreparedStatement 对象：prepareStatement();

​	③ Connection 产生 CollableStatement 对象：prepareCall();



Statement 操作数据库对象：

​	增删改：executeUpdate();

​	查询：executeQuery();

ResultSet：保存结果集	如：select * from 表名

​	next(); 光标下移，判断是否有下一条数据：true/false

​	previous()：true/false

​	getXXX(字段名|位置)：获取具体的字段值



处理CLOB/BLOB类型

处理稍大型数据：

​	① 存储路径	E:\JDK_API_zh_CN_CHM

​	通过JDBC存储文件路径，然后根据IO操作处理

​	例如：JDBC将 E:\JDK_API_zh_CN_CHM 文件，以字符串形式 " E:\JDK_API_zh_CN_CHM" 存储到数据库中

​	获取：	1.获取该路径"E:\JDK_API_ZH_CN_CHM"

​			2.IO

​	②

​		CLOB：大文本数据类型（小说 --> 数据）

​		BLOB：二进制