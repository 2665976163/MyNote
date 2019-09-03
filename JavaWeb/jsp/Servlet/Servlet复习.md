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
 ServletConfig getServletConfig()：获得一个ServletConfig对象
 String getServletInfo() ：
```



我们刚开始实现Servlet接口就可以简单的接收客户端发送过来的请求，但重写的方法数不胜数，若只想使用某些功能可以使用Servlet的实现类如下：

​	GenericServlet：该实现类除了service没有实现其余全都实现了，当继承该类时无需重写其他方法

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

​	<load-on-startup></load-on-startup>

标签内可以加任意数字，数字越小优先级越高，建议不要是负数从1开始

​	<load-on-startup>1</load-on-startup>

若加上以上这段标签则多个Servlet时就都可以执行初始化.





推测：

​	为什么初始化就只会初始化一次呢？可能内部做了一些操作，比如设了一个变量=true，若有请求的时候会判断是否要请求的资源是否实现了Servlet若实现了Servlet则判断开始的变量是否为ture，若为true则执行请求的Servlet，然后将变量改为false，那么之后请求的Servlet资源就不会再初始化了。







## ServletConfig

主要功能

​	获取客户端发起请求对应的Servlet在web.xml中的<servlet>标签中的内容.

比如：我写了一个ShowServlet的类，若使用2.0则先得注册Servlet，然后客户端发送请求访问我这个ShowServlet，那么我就得去web.xml找这个对应的请求资源，那么在web.xml首先找<servlet-mapping>

中的url-pattern，若请求与pattern一致，就通过mapping中的name找到servlet中的name，然后执行servlet中的class，那么ServletConfig就是用于获取mapping中name找到的servlet标签中的一些内容.

> 获取ServletConfig对象

```java
通过继承调用父类的getServletConfig()获得ServletConfig的实例;
```

> 常用方法

```java
Enumeration getInitParameterNames()：获取一组name值
String getInitParameter(String name)：通过name值获取value值
String getServletName()：获取当前servlet标签中的name
ServletContext getServletContext()：获取一个ServletContext对象
```



**为什么需要ServletConfig？**

​	因为若有一些技术咱们不会写，但有人提供了，他将操作都写入了Servlet中，但Servlet内需要一个参数，值是不固定的，而这个参数就写在Servlet标签中的init-param中，得需要读取servlet标签内的数据，那么就需要ServletConfig了，因为ServletConfig可以获取Servlet标签内的一些数据，如[init-param]、[servlet-name]



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

比如数据库的连接信息.







## ServletContext

1. 获取全局配置参数
2. 获取web工程中的资源
3. 存取数据，Servlet间共享数据，域对象

```xml
<!-- xml文件声明全局数据 -->
<context-param>
    <param-name>123</param-name>
    <param-value>234</param-value>
</context-param>
```

> 获取 ServletContext 对象

```java
在Servlet中调用 ServletContext() 方法 获取 ServletContext 对象.
```

> 获取xml中的值方法

```java
getInitParameter(String name) //通过name获取value
```

> 常用方法

```java
 Object getAttribute(String name) 
          返回具有给定名称的servlet容器属性，如果没有该名称的属性，则返回null。
 void removeAttribute(String name) 
          从servlet上下文中删除具有给定名称的属性。
 void setAttribute(String name, Object object) 
          将对象绑定到此servlet上下文中给定的属性名。
 URL getResource(String path) 
          返回映射到指定路径的资源的URL。
 InputStream getResourceAsStream(String path) 
          以InputStream对象的形式返回位于指定路径上的资源。
```

### 生命周期

> ServletContext 诞生与销毁

```java
tomcat启动时，会为托管的每一个web应用程序创建一个ServletContext对象.
从服务器移除托管 或者 关闭服务器.
```

> ServletContext作用范围

```java
整个项目有效，因为整个项目就算是一个web应用程序，整个项目中不管通过什么方式获取ServletContext实例，其实都是同一个ServletContext.
```







### 路径问题

在Web项目中若使用相对路径可能无法正常运行，因为最终项目是交给Tomcat的，Tomcat只会加载src下的资源。并不会加载src，若在web中使用相对路径获取的是项目的相对路径，而不是tomcat中加载的相对路径，当请求发起时执行的是tomcat内的class文件，而class内file中中却写着src/xxx.properties，那么这样肯定是找不到的，因为没有src.

**解决方案一**：ServletContext

ServletCotext中有方法可以获取到tomcat中部署项目的根目录，然后加上文件所在位置的相对目录就可以成功获取到资源了.

> ServletContext 获取资源文件路径

```java
public String getRealPath(String path) //path 写相对路径,若为""则为项目根目录
```

> ServletContext 获取资源文件路径

```java
public java.io.InputStream getResourceAsStream(String path) //与以上一样，但会返回一个流对象
```



**解决方案二**：ClassLoder

ClassLoder是java内置的，可以通过Class对象获取，ClassLoder可以获取一些该Class对象所在的路径，还有一些其他的信息。

> ClassLoder  获取资源文件路径 [String]

```java
this.class.getClassLoder().getResource(String path).getPath();  //返回的是path的绝对路径
```

> ClassLoder  获取资源文件路径 [InputStream]

```java
this.class.getClassLoder().getResourceAsStream(String path);  //返回用于读取指定资源的输入流
```







































## HttpServletRequest

将客户端发送请求的一切数据包装在该对象中  比如：Cookie、请求头、额外路径信息等。

> 常用方法

```java
Enumeration getHeaderNames(): 返回此请求包含的所有请求头的枚举。
String getHeader(String name): 以字符串的形式返回指定请求头的值。
Enumeration getHeaders(String name): 返回指定请求头的所有值。
[主要] String getParameter(String name) 根据name获取请求参数的value，若不存在则为null.
```

### Request get乱码问题

tomcat 7或之前的版本则默认编码ios-8859-1，7以后的版本默认编码UTF-8

当客户端向服务器发送请求，若使用7或之前的版本则会产生客户端向服务端发送请求信息，服务端解析出现乱码问题，因为客户端发送的数据若为get则会被浏览器编码后在地址栏显示，当通过tomcat解析这个请求时默认使用ios-8859-1编码解析，那么服务端获取中文请求的话就是一个乱码数据，因为utf-8编码 ios-8859-1解码钥匙不对门。

get乱码问题只针对7或之前版本，8或以上无需担心乱码问题，因为默认使用UTF-8解码

> 解决方式 一

```java
//将解码成ios-8859的数据再用ios-8859编码回去那么就是开始客户端发送请求的utf-8的字节数据了
byte[] data = requestId.getBytes("ios-8859-1");
//再将该字节数据用UTF-8创建出来
new String(data,"UTF-8");
```

> 解决方式 二

```java
在config目录中的servlet.xml的端口号那一行标签内加入属性URIEncoding="UTF-8";
那么就将tomcat中的默认编码改为了UTF-8	//若不想使用ios、utf-8 任何版本都可以这样修改默认编码
```

### Request post乱码问题

post 请求不会在地址栏显示，但若出现乱码解决方案如下

```java
request.setCharacterEncoding("UTF-8");
```

















## Response

响应对象.

> 常用方法

```java
 void setHeader(String name, String value): 设置具有给定名称和值的响应标头。
 void setStatus(int sc): 设置此响应的状态代码。
 =========================================================================
 ServletOutputStream getOutputStream(): 返回一个适合在响应中写入二进制数据的ServletOutputStream。
 PrintWriter getWriter(): 返回一个PrintWriter对象，该对象可以向客户机发送字符文本。
 =========================================================================
 void setContentType(String type) 如果尚未提交响应，则设置发送到客户机的响应的内容类型。
 void setCharacterEncoding(String charset): 设置响应编码，例如UTF-8  //当使用response流时可能需要
```









### 小案例

#### 页面跳转

> 需要方法

```java
 void setHeader(String name, String value) : 设置具有给定名称和值的响应标头。
 void setStatus(int sc) : 设置此响应的状态代码。
```

> 案例

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp){
    resp.setStatus(302); //状态码值 
    resp.setHeader("Location","地址");
}
```



#### 响应客户端

> 需要方法

```java
OutputStream os = resp.getOutputStream();  //ServletOutputStream 字节流
Writer writer = resp.getWriter();	//printWriter 打印流
```

以上俩种输出流都可以向客户端输出数据，跟平常的Syso一致，只不过一个是控制台，一个是在网页中.

> 响应

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp){
	resp.setCharacterEncoding("US-ASCII");
	OutputStream ops = resp.getOutputStream();
	ops.write("<script>alert('login_false')</script>".getBytes("US-ASCII"));
}
//结果： 在页面中弹出警告框alert 内容：login_false 
```



#### 设置浏览器查看编码

客户端乱码 post

> 需要方法

```java
void setHeader(String name, String value) : 设置具有给定名称和值的响应标头。
```

> 案例

```java
response.setHeader("Content-type","text/html;charset=UTF-8");
response.setEncoding("UTF-8");
//使用以上这种方式的话，浏览器就会根据Servlet中指定的编码进行解码
```



#### 设置文字编码

客户端乱码 post

> 需要方法

```java
void setContentType(String type) 如果尚未提交响应，则设置发送到客户机的响应的内容类型。
```

> 案例

```java
response.setContentType("text/html;charset=UTF-8");
//设置响应的数据类型是html文本，并告知浏览器使用UTF-8解码
```





#### 客户端弹出下载

> Servlet界面

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp){
	String file = req.getParameter("file");
	resp.setHeader("Content-Disposition", " filename="+file);
    //↑ 该作用就是让浏览器弹出下载提示框，而并非直接显示.
	String path = getServletContext().getRealPath("/lib/qqfm_setup.exe");
	System.out.println(path);
	InputStream fis = new FileInputStream(path);
	OutputStream os = resp.getOutputStream();
	int len = 0;
	byte[] data = new byte[204800];
	while((len = fis.read(data))!=-1) {
		os.write(data,0,len);
	}
	os.close();
	fis.close();
}
```

> HTML界面

```html
<body>
	QQ -> <a href="Servlet_001?file=QQ.exe">下载</a><br/>
</body>
```

?前面是请求xml中的url-pattern中的值，后面是请求参数名=参数值

#### 中文下载

> Servlet 部分核心代码

```java
String clientType = request.getHeader("User-Agent");
if(clientType.equals("Firefox")){
    fileName = DownLoadutil.base64EncodeFileName(fileName);
}else{
    fileName = URLEncoder.encode(fileName, "UTF-8");
}
```

首先获取来访的客户端是什么，其次判断是否是火狐，若为火狐则使用该方式转码，否则使用另外一种方式转码











## Cookie

是一份小的数据，由服务器发送给客户端，然后存储在客户端的一份小数据.

### 应用场景

自动登陆、浏览记录、购物车.

### 为什么要有Cookie

Http的请求是无状态的。客户端与服务器通讯时是无状态的，就是说当我第二次再访问时，服务器根本就不知道我之前有没有访问过，为了更好的用户体验，更好的交互【自动登录】，从公司层面来说就是，为了更好的收集客户信息【大数据】

### Cookie的简单使用

> Servlet 片段

```java
Cookie cookie = new Cookie("key","value");
response.addCookie(cookie);
//当有客户端请求则将该cookie返回给客户端
//删除cookie
cookie.setMaxAge(0); //设为0，Cookie就立即清除了
```

> 常用方法

```java
 void setDomain(String pattern)：只有访问了指定域名才会带上Cookie
 void setMaxAge(int expiry)：设置cookie的最大年龄(以秒为单位)。
 void setPath(String uri)：只有访问了指定路径地址才会带上Cookie
 void setSecure(boolean flag)：指示浏览器是否只应使用安全协议(如HTTPS或SSL)发送cookie。
 void setValue(String newValue)：在创建cookie之后，为cookie分配一个新值。
```

> 举例
>

```java
cookie.setDomain(".znsd.com");
cookie.setPath("/CookieDemo");
```

















### 小案例

#### 自动登陆

目标：

客户端第一次登陆，第二次自动登陆。

> 使用方法

```java
 void setMaxAge(int expiry)：设置cookie的最大年龄(以秒为单位)。
 //该方法默认为-1 代表浏览器关闭则Cookie销毁 若为正数按秒计算.
```

> 案例

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp){
	//获取客户端的所有Cookie
    Cookie[] cookies = req.getCookies();
	resp.setContentType("text/html;charset=UTF-8");
    //若客户端带来的是默认cookie则给客户端发送Cookie
	if(cookies.length==1) {
		Cookie cookie = new Cookie("abc","12c");
         //设置Cookie 销毁时间.
		cookie.setMaxAge(500000);
		resp.addCookie(cookie);
		resp.getWriter().write("登陆成功！");
	}else {
		resp.getWriter().write("请登陆！");
		for(Cookie cookie:cookies) {
			System.out.println(cookie.getName()+"=="+cookie.getValue());
		}
	}
}
```



#### 获取上次登陆时间

> Servlet 代码片段

```java
//第一次登陆获取到了当前时间加入到cookie发送给客户端
Cookie cookie = new Cookie("张三",System.currentTimeMillis()+"");
//第二次登陆打印该客户端的Cookie
Cookie cookies = request.getCookies();
System.out.println(new Data(Integer.valueof(cookies[1].getValue)));
```

