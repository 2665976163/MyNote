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













## Session

**【概念】**

​	服务器会话技术，将一次会话多次请求间共享数据，将数据存储在服务器端的对象中，httpSession.

​	与cookie作用差不多，cookie主要将数据存储在客户端【不安全，用户在开发者工具中可以查看cookie中的

​	数据，若存在账户则可能发生密码泄露等问题】，而session将数据存储在服务端从而解决此问题，但终究还

​	是依赖于cookie.

​	

**【原理】**

​	在用户访问服务器端时，服务器端会默认给客户端分配一个默认的cookie对象，

​	对象名=JSESSIONID，值为随机生成【JSESSIONID=D89F8A428DFDD037BCA4123F2158BF07 】，

​	先前我们将用户的账号密码存储在用户那里，当用户来请求时，我们就从客户端读取先前存储的cookie中的	

​	账号密码，从而识别用户的身份，但现在用户的信息存储在服务端，session是怎么做到可以识别用户的？，

​	就是依赖于开始默认给客户端分配的cookie对象，首先获取客户端默认的cookie对象，通过该cookie的value

​	去session中找是否有该value对应的value，我们开始向session中存储数据的时候就是拿客户端默认的cookie

​	中value作为键，然后账号密码作为值，当用户多次请求时好可以获取到之前存储的数据，但前提是不能关闭

​	客户端，因为关闭客户端时再次请求时会重新分配默认cookie的值，导致数据在服务器存在，但客户端却拿

​	不到，解决方法就是将第一次请求的默认cookie保存起来，设置cookie的生命周期，返回给客户端，因为名

​	称一样，会覆盖客户端的默认Cookie值，当客户端再次请求时服务端就不会分配cookie对象给客户端了，因	

​	为开始已经给默认的cookie对象设置了生命周期了，等时间过了才会再次分配.













## 验证码

生成验证码格式

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
	int width = 100;
	int height = 50;
	// 创建验证码对象
	BufferedImage img = new BufferedImage(width,height,BufferedImage.TYPE_INT_RGB);
	// 美化图片，获取图像对象
	Graphics g = img.getGraphics();
	// 设置下一次使用的颜色
	g.setColor(Color.BLACK);
	// 填充验证码背景颜色
	g.fillRect(0, 0, width, height);
	// 设置下一次使用的颜色
	g.setColor(Color.CYAN);
	//  边框颜色 
	g.drawRect(0, 0, width-1, height-1);
	// 设置下一次使用的颜色
	g.setColor(Color.GRAY);
	// 设置验证码文字
	String str = "ABCDEFGHIJKMNOPQRSTUVWSYZabcdefghijkmnopqrstuvwd0123456789";
	Random r = new Random();
    // 设置文字位置，等随机文字
	for(int i=0;i<4;i++) {
		g.drawString(str.charAt(r.nextInt(str.length()))+"",(width/5)*i,25);
	}
    // 设置干扰线
	for(int i=0;i<10;i++) {
		int x1 = r.nextInt(width);
		int y1 = r.nextInt(width);
		int x2 = r.nextInt(height);
		int y2 = r.nextInt(height);
		g.drawLine(x1, y1, x2, y2);
	}
	// 将验证码图片返回给界面
	ImageIO.write(img, "jpg", resp.getOutputStream());
}
```

点击切换图片

```html
<script>
	window.onload = function(){
		var img = document.getElementById("sss");
		img.onclick = function(){
			var date = new Date().getTime();
			img.src = "/web_Demo_002/VCServlet?"+date;
		}
	}
</script>
<body>
	<img id="sss" src="/web_Demo_002/VCServlet" />
	看不清？瞎了？点击图片换一张吧
</body>
```













## jsp	







### jsp指令

**【作用】**

​	用于配置jsp页面，导入资源文件.

**【格式】**

```xml
<%@ 指令名称 属性名1=属性值1 属性名2=属性值2 ... %>
```

**【分类】**

​	1.page		: 配置jsp页面的

```java
> contentType : 等同于response.setContentType();
	1.设置响应体的mime类型以及字符集
	2.设置当前jsp页面源码（高级工具生效）
> improt : 导包
> errorPage : 当前界面发生异常自动跳转到指定界面
> isErrorPage : 标识当前是否为错误页面
	> false : 默认值,不能使用内置Exception
	> true : 可以使用Exception
```

​	2.include	: 引入其他页面的资源文件【类似于html的框架，引入其他界面资源】

```xml
<% @include file="?.jsp" %>  //通过该方式引入jsp与jsp之间会在同一个文件中，java变量可以互相调用
```

​	![include](images\include.png)

![include效果](images\include效果.png)



​	jsp:include	：引入其他页面的资源文件【类似于html的框架，引入其他界面资源】

```html
<jsp:include page="?.jsp" %>  //通过该方式引入tomcat底层会产生俩个servlet文件，变量不可以互相调用
```



​	3.taglib		: 导入资源【比如struts2的标签库】

```xml
<%@ taglib uri="/struts-tags" prefix="s" %>
```







### 注释

jsp中可以使用注释：html、jsp【java但仅限于代码片段中】

```html
<!--  --> html的注释，不能注释标签
```

```java
<%--  --%>  jsp的注释，可以注释所有内容.
```

**全局变量**

```java
<% String a = "123"  %>
```

**内部类、方法**

```java
<%! 
    class a{}
	public static void add(){}
%>
```











### 内置对象

使用时无需new，jsp内置的，可以直接调用.

**【目录】**

   	1. pageContext		------		PageContext				当前页面共享【可以获取其他8个内置对象】
   	2. request			------		HttpServletRequest		一次请求共享【请求转发】
   	3. session			------		HttpSession				一次会话共享
   		. application            ------		ServletContext			所有用户共享
   		. response		------		HttpServletResponse		响应对象
   		. page			------		this						当前servlet对象
   		. out			        ------		javax.servlet.jsp.JspWriter	输出对象，数据输出到页面上
   		. config			------		ServletConfig			servlet的配置对象
   		. exception		------		Throable				异常对象

前面的仅为变量名，而对应真实的类型为后面的，可以在jsp转为servlet中查看.







### MVC 开发模式

1. jsp演变历史

   1. 早期只有servlet,只能使用response输出标签数据,非常麻烦.
   2.  后来出现了jsp，简化了Servlet的开发，如果过度使用jsp，在jsp中即写大量的java代码，有写html表，造成难于维护，难于分工协作.
   3. 再后来，java的web开发，借鉴mvc开发模式，使得程序的设计更加合理性.

2. MVC

   1. M=	Model 模型	JavaBean.

      ​	\> 完成具体的业务操作，如：查询数据库、封装对象

   	. V=	View 视图	Jsp

      ​	\> 展示数据

   	. C=	Controller 控制器	Servlet

      ​	\> 获取用户的输入

      ​	\> 调用模型

      ​	\> 将数据交给视图显示

**【优缺点】**

 	1. 优点
      	1. 耦合性低，方便维护，可以利于分工协作
      	2. 重用性高
		2. 缺点
      	1. 使得项目架构变得复杂，对开发人员要求高













### EL 表达式

**【概念】** Expression Language 表达式语言.

**【作用】** 替换和简化jsp界面中java代码的编写.

**【语法】** ${表达式}

**【注意】**

​	jsp默认支持 EL表达式，若想忽略 EL表达式 则可以使用以下方法

​	\> 在jsp中page指令中添加 isELIgnored= "true" ，若为true则忽略该界面全部EL表达式

```xml
<%@ page language="java" contentType="text/html; charset=UTF-8" isELIgnored= "true"
    pageEncoding="UTF-8"%>
```

​	\> 使用转义符忽略单个EL表达式

```xml
\${}
```

**【使用】**
**1.运算**
	**【运算符】**
		1.算数运算符 +   -  *  /(div)  %(mod)

​		2.比较运算符	>  <  >=  <=  ==  !=	

​		3.逻辑运算符: &&(and) II(or) !(not)

​		4.空运算符: empty

​			用于判断字符串、集合、数组对象是否为null并且长度是否为0

​			有值为false，没值为true

​			${empty list}	若list为null或者长度为0则为true

​	

**2.获取值**

el表达式只能从域对象中获取值
	**\> 语法**

​	1. ${域名称.键名} :从指定域中获取指定键的值.

​		**\> 域名称**

​			pageScope --- pageContext

​			requeStscope --- request

​			sessionScope --- session 

​			applicationScope --- application (ServletContext)

**举例**

```java
 //在request域中存储了name=张三
 ${requestscope.name}; //返回一个张三
```

​	2. ${键名} : 从小到大找域的值，找到即返回.

**举例**

```java
${pageScope.setAttribute("abc","xjy1")}
${requestscope.setAttribute("abc","xjy2")}
${sessionScope.setAttribute("abc","xjy3")}
首先在三个域中都存储了相同的键
${abc}	//结果 xjy1 因为page域范围最小.	
```

#### **获取对象**

```html
<body>
	<%
		JavaBean bean = new JavaBean();
		bean.setUser("admin");
		bean.setPsWord("123");
		request.setAttribute("bean", bean);
	%>
	${requestScope.bean.user}<br />
	${requestScope.bean.psWord}<br />
</body>
```

#### **获取List集合**

```html
<body>
	<%
		List list = new ArrayList();
		list.add("abc");
		request.setAttribute("list", list);
	%>
	${list[0]}<br />
</body>
```

#### **获取Map集合**

```html
<body>
	<%
		Map map = new HashMap();
		map.put("abc","aabbcc");
		request.setAttribute("map", map);
	%>
	<!-- 俩种方式 -->
	${map.abc}<br />
	${map["abc"]}<br />
</body>
```

**获取map中对象的值**

```html
<body>
	<%
		Map map = new HashMap();
		JavaBean user = new JavaBean();
		user.setUser("root");
		user.setPsWord("123");
		map.put("user", user);
		request.setAttribute("map", map);
	%>
	<!-- 俩种方式 -->
	${map.user.user}<br />
	${map.user.psWord}<br />
</body>
```





#### 隐式对象

![EL11隐式对象](images\EL11隐式对象.png)







### JSTL

**【概念】**

​	JavaServer Pages Tag Library JSP标准 标签库，是由Apache组织提供的开源的免费的jsp标签	<标签>

**【作用】**

​	用于简化和替换jsp页面上的java代码
**【使用步骤】**

​	1. 导入jst1相关jar包

​	2. 引入标签库: taglib指令: <%@ taglib %>

​	3. 使用标签

#### IF

类似于java的IF.

使用格式.

```html
<c:if test="条件"></c:if>
<!-- 如果表达式为true，则显示if标签体内容，如果为false，则不显示标签体内容 -->
```

**【案例】**

```html
<!-- 导入标签库 -->
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!-- 代码片段 -->
<body>
	<%
		List list = new ArrayList();
		list.add("阿迪达斯");
		list.add("耐克");
		list.add("特步");
		list.add("乔丹");
		request.setAttribute("list", list);
	%>
	<c:if test="${empty list}">
		为空是真的
	</c:if>
	<c:if test="${not empty list}">
		不为空是真的
	</c:if>
</body>
```

注意

​	**\>** if标签没有else，想要else，则可以在定义一个if标签

​	**\>** 使用if标签时必须有test标签，test标签内为条件语句，test中可以使用EL表达式







#### choose

类似于java的switch.

使用格式.

```html
<c:choose>
	<c:when test="${requestScope.value == 1}">若值为1则显示</c:when>
    <c:when test="${requestScope.value == 2}">若值为2则显示</c:when>
    <c:otherwise>default值</c:otherwise>
</c:choose>
```

**【案例】**

```html
<!-- 导入标签库 -->
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!-- 代码片段 -->
<body>
	<%
		request.setAttribute("value", 1);
	%>
	<c:choose>
		<c:when test="${value == 1}">星期一</c:when>
		<c:when test="${value == 2}">星期二</c:when>
		<c:when test="${value == 3}">星期三</c:when>
		<c:otherwise>默认值</c:otherwise>
	</c:choose>
	<br/>
</body>
```







#### Foreach

类似于java的for循环.

使用格式.

```html
<!-- 导入标签库 -->
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!-- 代码片段 -->
<body>
	<c:forEach begin="1" end="10" var="i" step="2" varStatus="s" >
     <!-- begin:开始 end:结束 var:变量名 step:步增 varStatus:循环状态对象 -->
		${i}		<!-- 变量名的值 -->
		${s.index} 	 <!-- 下标 与 变量名的内容一致 -->
		${s.count} 	 <!-- 循环次数 -->
	</c:forEach>
</body>
```

迭代List集合

```html
<!-- 导入标签库 -->
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!-- 代码片段 -->
<body>
	<% 
		JavaBean bean1 = new JavaBean("admin01","123");
		JavaBean bean2 = new JavaBean("admin02","123");
		JavaBean bean3 = new JavaBean("admin03","123");
		JavaBean bean4 = new JavaBean("admin04","123");
		JavaBean bean5 = new JavaBean("admin05","123");
		JavaBean bean6 = new JavaBean("admin06","123");
		JavaBean bean7 = new JavaBean("admin07","123");
		JavaBean bean8 = new JavaBean("admin08","123");
		List list = new ArrayList();
		list.add(bean1);
		list.add(bean2);
		list.add(bean3);
		list.add(bean4);
		list.add(bean5);
		list.add(bean6);
		list.add(bean7);
		list.add(bean8);
		request.setAttribute("list", list);
	%>
	<table border="1">
		<c:forEach begin="0" end="${list.size()}" items="${list}" var="i" varStatus="s">
			<tr>
				<th>${i.user}</th>
				<th>${i.psWord}</th>
			</tr>
		</c:forEach>
	</table>
</body>
```

迭代map集合

```html
<c:forEach items="${map}" var="item">
${item.key.name}-${item.value}<br/>
</c:forEach>
```



















### 分页

> 公式

```java
//(前台传入，默认页面从1开始)
int page = 1;
//显示数据
int rows = 5;
//起始位置	
int start = (page - 1) * rows;	//若是第一页则start从集合的0位置开始
int end = page * rows; //若是第一页则end从集合的5位置结束
```

**工具类**

```java
public class PageSwitch<T> {
	private Integer start = 0;
	private Integer rows = 5;
	private Integer end = rows;
	private Integer page = 1;

	/* 初始化行列参数 第一执行*/
	public void initParm(HttpServletRequest req, HttpServletResponse resp) {
		String reqPage = req.getParameter("page");
		String reqRows = req.getParameter("rows");
		HttpSession session = req.getSession();
		/* 显示多少行数据 */
		if(reqRows != null) {
			rows = Integer.valueOf(reqRows);
		}
		/* 第几页 */
		if(reqPage != null) {
			page = Integer.valueOf(reqPage) ;
			start = (page - 1) * rows;
			end = page * rows;
		}
	}
	
	public List<T> limit(List<T> list){
		List<T> newList = new ArrayList();
		end = end>=list.size()?list.size():end;
		for (int i = start; i < end; i++) {
			newList.add(list.get(i));
		}
		return newList;
	}

	public Integer getStart() {
		return start;
	}

	public Integer getRows() {
		return rows;
	}

	public Integer getEnd() {
		return end;
	}

	public Integer getPage() {
		return page;
	}
}

```

```html
<a href="../QueryRoleServlet?page=${pages-1<1?1:pages-1}">上一页</a>
<c:forEach begin="1" end="${count}" varStatus="i">
	<a href="../QueryRoleServlet?page=${i.count}" id="index${i.count}">${i.count}</a>
</c:forEach>
<a href="../QueryRoleServlet?page=${pages+1>count?count:pages+1}"> 下一页</a>
<span aria-hidden="true">共${count}页</span>
```









### JNDI  

tomcat配置文件中存储数据，当取数据时根据存入写的指定类型取出，根据键取值..

**普通存值**

```xml
<!-- context.xml中 name为key type为取值类型 value为值 -->
<Environment name="tjndi" value="hello JNDI" type="java.lang.String" />
```

**取值**

```java
//javax.naming.Context提供了查找JNDI 的接口  注意 tomcat开启时才会创建context对象
Context ctx = new InitialContext();	
//java:comp/env/为前缀 固定的
String testjndi = (String)ctx.lookup("java:comp/env/tjndi");
out.println("JNDI: "+testjndi);
```





**连接池配置**

```xml
<Resource 	name="jdbc/news" 	
       		auth="Container" 	
          	type="javax.sql.DataSource"  
          	maxActive="100"  	
       		maxIdle="30" 		
          	maxWait="10000"  
          
          	username="root"   
          	password="123" 
      		driverClassName="com.mysql.jdbc.Driver"  
      		url="jdbc:mysql://localhost:3306/news"
/>
<!-- name为取值用的键 -->
<!-- auth指定管理Resource的容器，当前是tomcat -->
<!-- type为连接池的数据源全路径 -->
<!-- maxActive 最大活跃数 -->
<!-- maxIdle 最大空闲数 -->
<!-- maxWait 最大等待超时时间 -->
```













### 文件上传

**需要导入jar包**

```txt
commons-fileupload-1.2.2.jar和commons-io-2.4.jar
```

表单form中请求方式必须是post，还必须加上一个属性 enctype="multipart/form-data"

```xml
<form enctype="multipart/form-data" method="post">
```

需要了解的类	FileItemFactory

```java
public void setSizeThreshold(int sizeThreshold)	设置内存缓冲区的大小
public void setRepositoryPath(String path )	设置临时文件存放的目录
```

需要了解的类	ServletFileUpload

```java
public void setSizeMax(long sizeMax)	
    		设置请求信息实体内容的最大允许的字节数
public List parseRequest(HttpServletRequest req)
    		解析form表单中的每个字符的数据，返回一个FileItem对象的集合
public static final boolean isMultipartContent (HttpServletRequest req )	
    		判断请求信息中的内容 是否是“multipart/form-data”类型
public void setHeaderEncoding(String encoding)	
    		设置转换时所使用的字符集编码
```

需要了解的类  FileItem

```java
public boolean isFormField()	
    		判断FileItem对象封装的数据类型（普通表单字段返回true，文件表单字段返回false）
public String getName()	
    		获得文件上传字段中的文件名（普通表单字段返回null）
public String getFieldName()	
    		返回前台提交表单字段元素的name属性值
public void write()	
    		将FileItem对象中保存的主体内容保存到指定的文件中
public String  getString()	
    		返回前台提交表单的value类似于getparm..()
public String  getString(String encoding)
    		返回前台提交表单的value,参数用指定的字符集编码方式
public long getSize()	
    		返回单个上传文件的字节数
```











**【案例演示】**

表单中只有文件，无任何其他参数

```html
<body>
    <form action="fileServlet" enctype="multipart/form-data" method="post">
    	<input type="file" name="file" />
        <input type="submit">
    </form>
</body>
```

fileServlet界面【注意案例演示的描述】

```java
public void doPost(HttpServletRequest req,HttpServletResponse resp){
    req.setCharacterEncoding("UTF-8");
	String filePath = getServletContext().getRealPath("/file");//设置上传的临时文件存放位置
	DiskFileItemFactory ff = new DiskFileItemFactory();
	ff.setRepository(new File(filePath));	
	ServletFileUpload fileUplod = new ServletFileUpload(ff);
  
    List<FileItem> list = fileUplod.parseRequest(req);
    for(FileItem item:list){
        item.write(new File(filePath,item.getName()));
    }
}
```

parseRequest：将上传表单中的input一个一个装进FileItem



一个标准的有其他参数，有文件的文件上传

**思路**

获取提交表单的全部参数 fileUplod.parseRequest(req);

根据FileItem中的方法判断自己是否为文件，若为文件则false，若为参数为true  fileItem.isFormField()

若为文件则存储，若为参数则取出

```java
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp){
	req.setCharacterEncoding("UTF-8");
	String filePath = getServletContext().getRealPath("/file");
	DiskFileItemFactory ff = new DiskFileItemFactory();
	ff.setRepository(new File(filePath));
	ff.setSizeThreshold(1024*10);
	ServletFileUpload fileUplod = new ServletFileUpload(ff);
	try {
		List<FileItem> list = fileUplod.parseRequest(req);
		for (FileItem fileItem : list) {
			if(fileItem.isFormField()) {
				System.out.println(fileItem.getFieldName() +"  "+fileItem.getString());
			}else {
				filePath = filePath+"/"+fileItem.getName();
				System.out.println("图片"+fileItem.getName());
				System.out.println(filePath);
				File file = new File(filePath);
				try {
					fileItem.write(file);
				} catch (Exception e) {
					e.printStackTrace();
				}
				fileItem.delete();
			}
		}
	} catch (FileUploadException e) {
		e.printStackTrace();
	}
}
```













### 状态码地址跳转

```xml
<error-page>
  	<error-code>404</error-code> //当状态码为404时跳转到localtion中的地址中
  	<location>/err.jsp</location>
</error-page>
```







### 页面异常跳转

可能发生异常的界面

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" errorPage="erro.jsp"%> 设置errorPage 当异常发生时自动跳转到指定界面
```

异常处理界面

```html
<body>
	请联系管理员 xxx ...
</body>
```



















### Filter 过滤器

​	平常用户直接可以访问目标路径，但出现过滤器后想访问目标路径首先背后会被过滤器拦截，然后经过一些操作之后再选择性判断你是否满足访问目标路径的条件，若不满足则做其他操作.

**【创建过程】**

​	**\>** 实现 Filter 接口	**javax.servlet.Filter**

​	**\>** 配置过滤器信息 注解 || web.xml

​	**\>** 是否放行 filterChain.doFilter(request, response)

**注意：** 若出现俩个过滤器过滤的是相同的内容则类名Unicode值小的先执行 

```java
// 举例
@webFilter("/*")	//AFilter 的过滤内容
@webFilter("/*")	//BFilter 的过滤内容
//执行A过滤器内容，因为A比B小
```



**【注解案例】**

```java
@WebFilter("/*")  // 注解方式声明过滤器
public class MyFilter implements Filter{

	@Override
	public void doFilter(ServletRequest request, 
                         ServletResponse response, FilterChain filterChain){
		System.out.println("拦截咯");
         //放行
		filterChain.doFilter(request, response);
	}
}
```

**【XML案例】**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
  </welcome-file-list>
     <filter>
        <filter-name>filter</filter-name>
        <filter-class>Filter.MyFilter</filter-class>	<!-- 过滤器类 -->
    </filter>
    <filter-mapping>
        <filter-name>filter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```





#### 执行流程

 	1. 执行过滤器
		2. 执行放行代码
		3. 放行代码执行完毕后再回来执行未执行的代码.

**【案例】**

```java
public void doFilter(ServletRequest arg0, ServletResponse arg1, FilterChain arg2){
		System.out.println("拦截咯");
		arg2.doFilter(arg0, arg1);	// 放行
		System.out.println("执行完放行后的资源回来再打印当前语句");
}
```









#### 生命周期

1. init:在服务器启动后，会创建Filter对象， 然后调用init方法。只执行-次。用于加载资源
2. doFilter:每一次请求被拦截资源时，会执行。执行多次
3. destroy:在服务器关闭后，Filter对象被销毁。如果服务器是正常关闭，则只会执行一次。用于释放资源







#### 过滤器配置详解

##### 拦截路径配置

​	拦截器只拦截指定或部分资源.

**【四种情况】**

​	写在过滤器的注解内或xml配置中就可以了

**1. 具体资源路径: /index.jsp**

​	只有访问index. jsp资源时，过滤器才会被执行

**2. 拦截目录: /user/***	模糊拦截【有可能资源名称以/user/*开始，又可能真实存在user目录】

​	访问/user下的所有资源时,过滤器都会被执行

**3. 后缀名拦截: *.jsp**	模糊拦截

​	访问所有后缀名为jsp资源时，过滤器都会被执行

**4. 拦截所有资源: /***

​	访问所有资源时，过滤器都会被执行

**【案例演示】**

```java
@WebFilter("/user/*")	
public class MyFilter implements Filter{
	@Override
	public void doFilter(ServletRequest arg0, ServletResponse arg1, FilterChain arg2){
		System.out.println("拦截咯");
		arg2.doFilter(arg0, arg1);	// 放行
	}
}

```

> Servlet

```java
@WebServlet("/user/AServlet")	
class AServlet extends HttpServlet{
    @Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp){
        System.out.println("当访问/user/AServlet不会直接访问到我而是执行拦截器");
	}
}
@WebServlet("/user/BServlet")	
class BServlet extends HttpServlet{
    @Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp){
        System.out.println("当访问/user/BServlet不会直接访问到我而是执行拦截器");
	}
}
```













##### 拦截方式配置

资源被访问的方式，若不配置则只有请求转发才能被过滤器拦截.

​	 **注解配置**

​		设置dispatcherTypes属性

​			REQUEST 默认值。浏览器直接请求资源【重定向】

​			FORWARD 请求转发访问资源

​			INCLUDE 包含访问资源

​			ERROR 错误跳转资源

​			ASYNC 异步访问资源

```java
@WebFilter(value="/*",dispatcherTypes= {DispatcherType.REQUEST,DispatcherType.FORWARD })
```

​	**web. xml配置**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <filter>
    <filter-name>struts2</filter-name>
    <filter-class>Filter.MyFilter</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>struts2</filter-name>
    <url-pattern>/index.jsp</url-pattern>
    <dispatcher>REQUEST</dispatcher> <!-- 核心 -->
    <dispatcher>FORWARD</dispatcher> <!-- 核心 -->
  </filter-mapping>
</web-app>
```





**【作业】**

 1. 判断用户是否登陆

    **\>** 用户在没进行登陆时访问需要登陆才能访问的资源时进行拦截让用户返回到登陆界面

    **\>** 但用户访问登陆时也会被拦截，当访问登陆时直接放行，获取访问路径判断是否包含登陆页面.

    **\>** 拦截器会拦截一切资源包含css、js所有资源，当访问页面包含这些也要进行判断放行.

























### Listenter 监听器

监听一个对象的诞生与销毁，对象的变化【增删查改】等.

**【监听器对象】**

- ServletContext
- HttpSession
- ServletRequest

以上三个对象包含许多监听器，以下举例几个.

**【过程】**

​	**\>** 实现监听器接口，重新接口方法

​	**\>**  注册监听器、注解或者xml

> 注解

```java
@WebListener
```

> xml

```xml
<listener>
  	<listener-class>listener.MyLister</listener-class>
</listener>
```

**【案例演示】**

> ServletContextListener

```java
@WebListener
public class MyLister implements ServletContextListener {

	@Override
	public void contextDestroyed(ServletContextEvent sce) {
		System.out.println("监听器关闭");
	}

	@Override
	public void contextInitialized(ServletContextEvent sce) {
		System.out.println("监听器初始化完毕");
	}
}
```

> ServletRequestListenter

```java
public class RequestListenter implements ServletRequestAttributeListener{

	@Override
	public void attributeAdded(ServletRequestAttributeEvent srae) {
		System.out.println("rquest中增加了一个参数");
	}

	@Override
	public void attributeRemoved(ServletRequestAttributeEvent srae) {
		System.out.println("rquest中删除了一个参数");
	}

	@Override
	public void attributeReplaced(ServletRequestAttributeEvent srae) {
		System.out.println("rquest中修改了一个参数");
	}

}

public class RequestListenter implements ServletRequestListener{
    @Override
	public void requestDestroyed(ServletRequestEvent sre) {
		System.out.println("销毁了一个request");
	}

	@Override
	public void requestInitialized(ServletRequestEvent sre) {
		System.out.println("诞生了一个request");
	}
}
```

> ServletContextListener

```java
@WebListener
public class SessionListenter implements HttpSessionListener {
	private int count;
    public void sessionCreated(HttpSessionEvent se)  { 
    	System.out.println("一个会话对象被创建了    当前用户="+(++count));
    	se.getSession().setAttribute("count", count);
    }
    public void sessionDestroyed(HttpSessionEvent se)  { 
    	System.out.println("一个会话对象被销毁了 当前用户="+(--count));
    	se.getSession().setAttribute("count", count);
    }
}
```

**注意：** session关闭浏览器后不会被销毁，默认30分钟后才会被销毁