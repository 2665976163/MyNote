# JDBC

一款java与数据库连接的jar包.

​	市面上数据库众多，每个数据库都有自己独特的语句，每个数据库提供的实现类皆不相同，若Java开发者想使用某一款数据库还得专门去查看该数据库对应实现类这显然是不公平的，因此sun公司定义了一套规范提供了一套数据库的api接口，若某款数据库想支持Java就必须提供实现类，因此Java开发者们只需要参考sun公司提供的接口就可以通过同一种方式使用不同的数据库了.



**【使用jar包】**

​	mysql-connector-java-5.1.44-bin.jar



**【常用的类】**

​	Driver、DriverManager、Connection、Statement、PreparedStatement、Result





### Driver

​	JDBC 驱动类，主要用于获取数据库连接对象Conn，但获取过于麻烦，被DriverManager封装了.

**【演示案例】**

​	获取Connection对象

​	**\> 步骤**

​		1 - 创建驱动类对象

​		2 - 创建配置文件对象，将账号密码存入

​		3 - 通过调用驱动类的Driver的connect方法将数据库url与配置文件对象传入返回结果为conn对象.

```java
//数据库账号、数据库密码、数据库url
String user = "root";
String psWord = "123";
String url = "jdbc:mysql:///test_01";
public static void main(String[] args) throws SQLException {
	Driver driver = new Driver();
	Properties info = new Properties();
	info.setProperty("user", user);
	info.setProperty("password", psWord);
	Connection conn = driver.connect(url, info);
}
```

​	一般不通过这种方式获取conn对象，因为过于麻烦，一般通过DriverManager获取conn对象.











### DriverManager

​	Driver的封装工具类，主要用于获取数据库连接对象Conn，简化了Driver中录入配置信息的操作.

**【演示案例】**

​	获取Connection对象

​	**\> 步骤**

​		1 - 加载驱动类

​		2 - 通过类名调用该类中的getConnection方法录入信息，获取conn对象.

```java
//数据库账号、数据库密码、数据库url
String user = "root";
String psWord = "123";
String url = "jdbc:mysql:///test_01";
public static void getConn() throws SQLException {
    Class.forName("com.mysql.jdbc.Driver");
	Connection conn = DriverManager.getConnection(url,user,psWord);
    return conn;
}
```



**为什么要通过Class.forName加载驱动类？**

注意：通过Class.forName加载类时该类的所有静态数据会被加载

​	Driver类有一个静态代码块，当使用Driver时静态代码块中会向DriverManager中注入一个Driver的实例，DriverManager只是Driver的一个封装工具类，内部是需要一个Driver的实例，所以通过反射的方式让Driver中的静态代码块向封装工具类中注入Driver的实例.



当然也可以不用反射的方式，可以直接new Driver也可以启动该类中的静态方法，但new 的时候产生一个Driver实例，静态代码块中也会new 一个Driver的实例，因此产生了俩个Driver实例，较耗费资源，所以不推荐.









### ResultSet

​	执行查询产生的结果集对象

> 常用方法

```java
public boolean next();	//指针往下走一步
public String getString([int|String]); //可以传列下标位置，可以传列名
public int getInt([int|String]); //...等其他基本类型的get方法
public Object getObject([int|String]);
```

> 举例

遍历查询的所有结果集

> 表结构

``` java
sid    sname    sage  //<- 默认指针在这个位置
 1      张三     12   // <- 调用一次next就移动到这个位置，可以获取当前这一行的所有数据
 2      李四     13
 3      王五     14
```

select * from student;

可以传入想获取的指定列名

```java
Statement stat = conn.createStatement();   //数据库增删查改对象 Conn中有描述
Result result = stat.executeQuery("select * from student");
while(result.next){
    stat.getInt("sid");
    stat.getString("sname");
    stat.getInt("sage");
}
```

也可以传入下标位置获取结果集字段从1开始 sid列 的下标为 1 sname列 为 2 以此类推

```java
Statement stat = conn.createStatement();
Result result = stat.executeQuery("select * from student");
while(result.next){
    stat.getInt(1);
    stat.getString("sname");
    stat.getInt(3);
}
```













### Connection

​	数据库连接对象 

​	用于数据库的一些操作比如：获取sql增删查改对象，事务等.

​	**【演示案例】**

​		**\>**  增删查改

​		**\>**  事务



#### Statement

一个用于增删查改等sql语句操作的对象

> 获取方式

```java
public Statement getStat(){
	Connection c = getConn();
    Statement stat = conn.createStatement();
}
```

> 常用方法

```java
public int executeUpdate(String sql);  //增删改都可以调用
public ResultSet executeQuery(String sql);  //查询调用，将结果封装到ResultSet中
```

**【案例演示】**

```java
public void add(int id,String name,int age){
    String sql = "insert into student values("+id+",+"name"+,"+age+")";
    int count = stat.executeUpdate(sql);
    System.out.println("影响行数=="+count);
}
public void remove(int id){
    String sql = "delete from student where sid = "+id;
    int count = stat.executeUpdate(sql);
    System.out.println("影响行数=="+count);
}
public void update(int id,String name){
    String sql = "update student set sname = '"+name+"' where sid = "+id;
    int count = stat.executeUpdate(sql);
    System.out.println("影响行数=="+count);
}
public void query(int user,String passWord){
    String sql = "update student set sid= '"+name+"'"+
        		"where sid = "+user+" and sname='"+psWord+"'";
    ResultSet resultSet = stat.executeQuery(sql);
    While(resultSet.next()){
        int user = resultSet.getInt("sid");
        String sname = resultSet.getString("sname");
        int age = resultSet.getInt("sage");
        System.out.println("账号:"+user+"密码:"+sname+"Q龄"+age);
    }
}
```

执行sql中通过外界传值执行语句，但存在安全隐患，那就是sql注入

##### sql注入

黑客并不清楚账号密码，通过另一种方式传递参数获取所有账号密码以及信息

```java
query(22111,"123' or 1=1");
通过这种方式就可以获取全部信息,因为即使sid与sname条件不满足，但是在后面拼接了一个or 1=1 代表只要数据库中有数据就满足条件。这样就将所有数据都获取出来了.
```

所以一般不使用statement，因为会发生sql注入，于是就出现了PreparedStatement 用于取代statement，该类利用了占位符技术解决了sql注入问题.















#### PreparedStatement 

与Statement用法大同小异，但使用了占位符技术，将原本直接写在sql语句中的值使用?代替.

当用户多次执行重复sql时，PreparedStatement 是要比Statement 性能高的，以为前者使用预编译，一次编译多次运行，但当用户若只执行单次的话前者的性能并不是很高，因为第一次需要编译sql.

> 创建方式

```java
public PrepareStatement getPs(){
    String sql = "insert into student values(?,?,?)";
	conn.PrepareStatement(sql);
}
```

> 常用方法

```java
public void setString(int parameterIndex,String x); //八大基本类型..等
public void setObject(int parameterIndex,Object x)
```

parameterIndex：第几个占位符

x：对应的参数值



**【案例演示】**

```java
public void test(){
    PrepareStatement ps = getPs();
    ps.setInt(1,4);	//第一个?占位符 替换为 4
    ps.setString(2,"雷震子")  //第二个?占位符 替换为指定值
    ...
    ps.executeUpdate();   //执行
}
```

```mysql
insert into student value(?,?,?);
insert into student value(1,"雷震子",28); -- 对应的参数值
```













#### ResultSetMetaData

获取表结构

> 创建方式 

```java
public ResultSetMetaData getRSD(){
    String sql = "insert into student values(?,?,?)";
	PreparedStatement ps = conn.PrepareStatement(sql);
	return ps.getMetaData();
}
```

> 常用方法

```java
String getColumnClassName(int column)	//获取指定列的Class全路径以字符串类型返回
int getColumnCount() // 返回Result中的总列数
String getColumnName(int column) // 返回列名称
int getPrecision(int column) // 返回指定列的容量大小
String getTableName(int column) //获取指定列的表名称
int getPrecision(int column) //获取指定列的数据数量
```









#### 事务

**ACID**（**Atomicity**  **Consistency**  **Isolation**  **Durability** ）

​	**原子性：**保证一个事务不可分割，是一体的，要么都执行要么都不执行.

​	**一致性：**保证一个事务执行完毕后，前后的数据总体来说是没有变化的.

​	**隔离性：**事务之间是互不影响的

​	**持久性：**将内存中的数据存储到硬盘中.



利用Conn开启事务（创建connection对象时自动提交默认为开启状态(true)），一个事务只针对一个连接对象，就比如a与b都是连接对象，a关闭了自动提交，而b并不会关闭，因为一个事务只针对一个连接对象.

**【演示案例】**

```java
public void test(){
    try{
    	conn.setAutoCommit(false);  //false 禁用自动提交 默认为true
    	...数据操作
         conn.commit();
    }catch(Exception e){  //若某一条数据操作抛出异常则开始回滚.
        System.out.println("数据错误开始回滚...");  
        conn.rollback();
    }
}
```

> 回滚到指定区域

```java
conn.setAutoCommit(false);
PreparedStatement ps = conn.prepareStatement("insert into staff values(?)");
Savepoint s = null;
for(int i=10;i<50;i++) {
	ps.setInt(1, i);
	ps.executeUpdate();
	if(i==25) {
		s = conn.setSavepoint("a回滚记录点");
	}
}
conn.rollback(s); //回滚到i=25的时候
conn.commit(); //执行commit，在数据库中插入了10-25的数据
```



















#### 存储过程

**调用参数只包含 in 的**

```mysql
delimiter $$
create procedure deleteField(in parem INT)
begin
  delete from 表名 where id = parem;
end $$
```

**调用 in**

```java
public static void main(String[] args){
    //获取连接对象，此处省略
    //获取操作存储过程对象,prepareCall中传入{call 过程名[(?,?...)]}  花括号可以省略 
    CallableStatement call = conn.prepareCall("{call deleteField(?)}");
    call.setInt(1);
    call.execute();
}
```





**调用参数包含 in 与 out 的**

```mysql
delimiter $$
create procedure deleteField(in parem INT,out rtn INT)
begin
  delete from 表名 where id = parem;
  set rtn = 10;
end $$
```

**调用 Out**

```java
public static void main(String[] args){
    //获取连接对象，此处省略
    //获取操作存储过程对象,prepareCall中传入{call 过程名[(?,?...)]}  花括号可以省略 
    CallableStatement call = conn.prepareCall("{call deleteField(?,?)}");
    call.setInt(1);
    call.registerOutParameter(2,Types.INTEGER);	//设置返回类型
    call.execute();
    System.out.println(call.getInt(2)); //获取输出参数
}
```





**调用参数 包含 inOut**

```mysql
delimiter $$
create procedure amplifier(inout parem INT)
begin
	set parem = parem + 10;
end $$
```

**调用 InOut**

```java
public static void main(String[] args){
    //获取连接对象，此处省略
    //获取操作存储过程对象,prepareCall中传入{call 过程名[(?,?...)]}  花括号可以省略 
    CallableStatement call = conn.prepareCall("{call amplifier(?)}");
    call.setInt(1,10); //设置输入值
    call.registerOutParameter(1,Types.INTEGER);	//设置输出类型
    call.execute();
    System.out.println(call.getInt(1)); //获取输出参数 结果 20
}
```















