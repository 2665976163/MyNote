# Servlet

什么是servlet？

​	其实就是一个java程序，运行在我们的web服务器上，主要用于接收与响应 客户端的http请求 .

注意 ：	

​	Tomcat 就是一个 存储 Servlet 的 容器，Tomcat 主要建立了 客户端 与 服务器交互的桥梁，

当客户端向服务器发送请求时，Tomcat 就会执行对应的 Servlet.



## 配置Servlet(2.0)

​	都知道若想使用jsp的话直接就在webContext下写一个jsp文件就好了，首启动就设置web.xml ，

但servlet不同，因为若你不声明它为Servlet，那么tomcat就把你当作jsp交给浏览器，那么浏览器就

会默认把你当作html解析，所以我们现在就需要告诉tomcat我是一个servlet，这里是Servlet2.0的配置方式.

```xml
<!-- 第一步告诉用户可以通过浏览器怎样找到我这个Servlet-->
<!-- 当tomcat被请求时请求路径与pattern一致，则通过该mapping中的name找到注册servlet中的name -->
<servlet-mapping>
    <!-- 与注册servlet保持一致的name-->
    <servlet-name>DIYServlet</servlet-name>
    <!-- 浏览器访问该Servalet的名称，以/开头 xml中/代表localhost:80/Project/ -->
    <servlet-pattern>/DIYServlet</servlet-pattern>
</servlet-mapping>
<!-- 第二步告诉tomcat我是一个Servlet ：向tomcat注册Servlet -->
<servlet>
    <!--与mapping保持一致name-->
    <servlet-name>DIYServlet</servlet-name>
    <!-- Servlet文件的全路径 -->
    <servlet-class>com.znsd.servlet.DIYServlet</servlet-class>
</servlet>
```

执行过程：

​	首先用户发送请求时，首先会去web.xml中找<mapping-mapping> 中的 <Servlet-pattern> 判断用户发送的请求与我[pattern]是否一致，若为一致则通过mapping 中的 <servlet-name> 去 <servlet> 标签内一个一个找，看mapping中的name 是否有 与 servlet 中的name匹配的，若有则执行<servlet>标签中的<servlet-class>中的servlet，若开始在web.xml中没有找到文件，则会去webContext下找是否有这个请求的文件。



1.0：**比如：[localhost:80/Project/DIYServlet]**







## 自定义 Servlet.

![Servlet继承图](images\Servlet继承图.png)

> Servlet

```java
Servlet是一个接口，当中有
 void init(ServletConfig config)：只有第一个被执行的Servlet才会执行，且只执行一次
 void service(ServletRequest req, ServletResponse res)：所有收到的请求都在这里执行
 void destroy()：当tomcat被关闭时执行
 ServletConfig getServletConfig()：
 String getServletInfo() ：
```



我们刚开始实现Servlet接口就可以简单的接收客户端发送过来的请求，但重写的方法数不胜数，若只想使用某些功能可以使用Servlet的实现类如下：

​	GenericServlet：该实现类除了service没有实现其余全都实现了，当继承该类时无需重新其他方法

但是我们想一下，客户端发送的请求方式不管是post请求||get请求或者其他请求全交给service处理的话那么将会混乱，因为我post的请求与get的请求想要的处理结果是不同的，而若都交给service处理的话那么导致结果都是一样的，这肯定是不行的，所以又出现了一个实现类如下：

​	HttpServlet：该类主要将GenericServlet 的Service方法重写了，并解决了以上所有请求都是service处理的一个缺陷，重写的方法功能就是将客户端发送的请求根据判断转给不同的处理请求的方法，该类有很多特有的方法，

我们只用重写特有的方法就行了比如doget、dopost。



## 生命周期

```java
创建 --> 初始化 --> 服务 --> 销毁 --> 卸载
```

创建：当Servlet文件诞生时.

初始化[init]：当有一个Servlet被请求时就开始初始化了，仅一次，若再次请求其他servlet则不会再执行初始化.

服务[service]：当客户端向服务器发送一次请求时就会执行一次

销毁[destroy]：当tomcat被正常关闭时，会执行销毁

卸载



初始化默认是请求才执行，若初始化内容过多，用户请求响应就会慢，那么能不能在tomcat启动时就初始化？

可以的，在web.xml的servlet标签中加入一行标签

​	<load-on-stratup></load-on-stratup>

标签内可以加任意数字，数字越小优先级越高，建议不要是负数从1开始

​	<load-on-stratup>1</load-on-stratup>





推测：

​	为什么初始化就只会初始化一次呢？可能内部做了一些操作，比如设了一个变量=true，若有请求的时候会判断是否要请求的资源是否实现了Servlet若实现了Servlet则判断开始的变量是否为ture，若为true则执行请求的Servlet，然后将变量改为false，那么之后请求的Servlet资源就不会再初始化了。







## ServletConfig

主要功能

​	获取客户端发起请求对应的Servlet在web.xml中的<servlet>标签中的内容.

比如：我写了一个ShowServlet的类，若使用2.0则先得注册Servlet，然后客户端发送请求访问我这个ShowServlet，那么我就得去web.xml找这个对应的请求资源，那么在web.xml首先找<servlet-mapping>

中的url-pattern，若请求与pattern一致，就通过mapping中的name找到servlet中的name，然后执行servlet中的class，那么ServletConfig就是用于获取mapping中name找到的servlet标签中的内容.

> 常用方法

```java
Enumeration getInitParameterNames()：获取一组name值
String getInitParameter(String name)：通过name值获取value值
String getServletName()：获取当前servlet标签中的name
ServletContext getServletContext()：获取一个
```



**为什么需要ServletConfig？**

​	因为若有一些技术咱们不会写，但有人提供了，他将操作都写入了Servlet中，但Servlet内需要一个参数，值是不固定的，得需要读取servlet标签内的数据，而这个参数就写在Servlet标签中的init-param中，那么就需要ServletConfig了，因为ServletConfig可以获取Servlet标签内的一些数据，如[init-param]、[servlet-name]



> Servlet标签中加入初始化参数[init-param]

```java
<servlet>
    <servlet-name>abc</servlet-name>
    <servlet-class>servlet.LookServlet</servlet-class>
    <init-param>
    	<param-name>id</param-name>
    	<param-value>123</param-value>
    </init-param>
    <init-param>
    	<param-name>张三</param-name>
    	<param-value>李四</param-value>
    </init-param>
  </servlet>
```

在Servlet初始化操作或者服务操作时，想获取一些由外界提供的参数，就可以使用初始化参数，由开发者定义.