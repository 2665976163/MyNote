# Druid 连接池

Druid 中文名称 德鲁伊 	由阿里开发出来的一款开源项目

## Druid的优势

**一.  亚秒级查询** 

```txt
druid提供了快速的聚合能力以及亚秒级的OLAP查询能力，多租户的设计，是面向用户分析应用的理想方式 .
```

**二. 实时数据注入** 

```txt
druid支持流数据的注入，并提供了数据的事件驱动，保证在实时和离线环境下事件的实效性和统一性 .
```

***三. 可扩展的PB级存储*** 

```txt
druid集群可以很方便的扩容到PB的数据量，每秒百万级别的数据注入。即便在加大数据规模的情况下，也能保证时其效性 .
```

**四. 多环境部署** 

```txt
druid既可以运行在商业的硬件上，也可以运行在云上。它可以从多种数据系统中注入数据，包括hadoop，spark，kafka，storm和samza等 
```

**五. 丰富的社区** 

```txt
druid拥有丰富的社区，供大家学习
```





## Druid 开发

需求：druid-1.1.9.jar（德鲁伊连接池jar包）、mysql-connector-java-5.1.44（mysql JDBCjar包）.



目标类：

​	  Druid中的数据源类：DruidDataSource

> 使用方式

```java
public static void main(String[] args) throws SQLException {
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setDriverClassName("com.mysql.jdbc.Driver");
        druidDataSource.setUrl("jdbc:mysql://localhost:3306/testBase");
        druidDataSource.setUsername("root");
        druidDataSource.setPassword("123");
    	//以上数据源就配置好了，可以直接给dbutils了，但不想使用则可以以下操作获取conncation对象做操作.
        Connection connection = druidDataSource.getConnection();
        System.out.println(connection);
}
```



​	