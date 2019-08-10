### 课程回顾

​	jsp底层就是一个servlet ，以前没有jsp的时候写的就是servlet但servlet写网页太麻烦了，所以诞生了jsp

jsp可以转为servlet，servlet 可以转为 jsp

当客户端对tomcat发请求时，tomcat响应并将该文件编译 成 java文件，然后运行 java 文件产生一个.class文件

所以若是大型网站编译会很慢（比较慢），但是只是针对第一次请求，出发用户对文件进行了修改，因为会再次

编译加载修改过的数据.



### 					JSP 的 页面元素

#### 1. 脚本 (Scriptlet)

若想在jsp界面写java代码则需要使用脚本语言

​		① <% 可以写局部变量 java语句  %>

​		② <%! 全局变量，定义方法 %>

​		③ <%= 输出表达式 %>

**注意**：一般修改web.xml 配置文件 .java 需要重启tomcat 服务器， 但若修改 jsp/html/css/js 则不需要重启



#### 2. 指令

​	Page 指令 <%@page ... %>

​		Page 中的属性：

​			language：jsp页面使用的脚本语言

​			import：导入类

​			pageEncoding：jsp文件自身编码 jsp -> java

​			contentType：浏览器解析jsp的编码

> 举例

```java
<%@page language="java" contentType="text/html;charset=UTF-8" import="java.lang.String">
```



#### 3. 注释

​	①	html注释 <!-- --> 源码可见

​	②	java注释  //  /*  */

​	③ 	注释 <%-- --%>





#### 4. JSP九大内置对象

​		（不需要new，也能使用的对象）

##### 	①	Out：输出对象

​			向客户端(浏览器)输出内容

##### 	②	request：请求对象

​			用于存储"客户端向服务端发送的请求信息"

​		常用方法：

​			String getParameter(String name)：根据请求的字段名key返回字段值value [根据name 拿 value]

​			String[] getParamenterValues(String name)：根据key，返回多个字段value [根据name 拿 value]

​			void setCharacterEncoding("编码值")：设置请求编码

​				(Tomcat 7以前默认编码为：IOS-8859-1  8以后为UTF-8)

​			getServerContext()：获取项目的ServletContext 对象

​			getRequestDispatcher("b.jsp").forward(request,resonse); 请求转发

​		注意：若想Tomcat get请求编码不是 IOS-8859-1 则在修改端口号旁边加入URIEncoding = "UTF-8"

##### 	③ response：响应对象

​		提供方法：

​			void addCookie("Cookie cookie");	服务端向客户端增加Cookie 对象

​			void sendRedirect(String location)throws IOException	重定向

​			void setContextType(String type);	设置服务端响应的编码（设置服务端的contextType类型）

![request](iamges\request.png)

过程：

```java
请求转发：
	当客户端发送请求给服务器指定网页，网页进行处理后直接转发给最终目标页，再响应客户端，当中只产生了一次请求一次响应.
重定向：
	当客户端发送请求给服务器指定网页，网页进行处理后立马响应客户端让客户端重新发送一次请求到另一个界面响应，当中产生了俩次请求，俩次响应
```

 补充：Cookie(客户端，不是内置对象)：Cookie是由服务端生成再发送给客户端保存.

​	相当于本地缓存的作用：客户端(s.mp4)  --> 服务端(s.mp4)

​	作用：提高访问服务器的效率，但安全性较差

##### Cookie：name =value

​	javax.servlet.http.Cookie	(类路径)

​	public Cookie(String name , String value);

​	String getName();

​	String getValue();

​	void setMaxAge(int exping)：最大有效期(秒)

服务端准备Cookie：resonse.addCookie(Cookie cookie);

​	页面跳转（转发、重定向）

客户端获取Cookie：request.getCookies();

​	① 服务端增加Cookie：response对象；客户端获取对象 request 对象

​	② 不能直接拿到某一个单独的Cookie，只能一次性获取全部

注意：通过 F12 可以发现 除了自己设置的Cookie 对象外，还有一个name为JSEssionID的Cookie对象

​	建议Cookie 只保存 英文数字 否则需要编码 解码



##### ④ session：会话对象

​	① 存储在服务端

​	② session 是在同一个用户（客户）请求时共享

​	③ 实现机制：第一次客户端请求时 产生一个sessionID 并赋值给Cookie 的Id 完成匹配

​	常用方法：

​		String getId()；获取sessionId

​		boolean isNew()：判断是否是新用户(第一次访问)

​		void invalidate()：使session 失效（退出登陆，注销）

​		

​		void setAttribute()：（name-value）设置sessionID值

​		Object getAttribute()：获取session值

​		

​		void setMaxInactiveInterval(秒)：设置最大有效非活动时间

​		int getMaxInactiveInterval()：获取最大有效非活动时间

![request](iamges\Cookie.png)

##### ⑤application：全局对象

​		String getContextPath()：虚拟路径

​		String getRealPath(String str)：利用以上获取虚拟路径的绝对路径

​		

##### JSP的四种范围对象

pageContext JSP页面容器 (Page对象)	当前页面有效

request 请求对象						同一次请求有效

session 会话对象						同一次会话有效

application 全局对象					全局有效（整个项目有效）

以上4个对象共有方法：

​	Object getAttribute(String name)：根据 name 获取 value

​	void setAttribute(String name,Object obj)：设置属性值（新增，修改）

​		若 name 不存在则创建name ，将obj 值赋给 name

​		否则修改name的值

​	void removeAttribute(String name)：根据属性名，删除属性对象 

① PageContext	当前页面有效（页面跳转则无效）

② request 当前请求（同一次请求有效，其他请求无效（请求转发有效，重定向无效））

③ session 同一次会话有效（无论怎么跳转都有效，关闭/切换浏览器后无效）

④ application 整个项目运行期间都有效（切换浏览器仍然有效）

​	--> 多个项目共享，重启后仍然有效：JNDI ⑤

以上范围对象，尽可能使用最小范围，因为对象的范围越大，造成的性能损耗越多.