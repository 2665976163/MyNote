### InetAddress

> 概念

```java
此类表示Internet协议（IP）地址，可以获取ip地址、主机名、等..
```

> 注意

```java
没有构造方法的类一般要么使用静态方法创建对象，要么就是工具类.
```

> InetAddress 常用方法

```java
getHostAddress();  //返回主机名
getHostName();	//返回主机地址
getCanonicalHostName()  //返回主机与地址名
```

> InetSocketAddress 

> 端口(用于区分软件 端口号：2字节 0-65535 ) 查看

```java
查看所有端口：netstat -ano
查看指定端口：netstat -ano|findstr "端口号"
查看指定进程：tasklist|findstr "进程号"
查看具体程序：使用任务管理器详细信息处.
```

> URL

```java
统一资源定位符，对可以从互联网上得到的资源的位置和访问方法的一种简洁的表示，是互联网上标准资源的地址。互联网上的每个文件都有一个唯一的URL。
```

> 创建对象

```java
URL url = new URL(String spec); 
```

