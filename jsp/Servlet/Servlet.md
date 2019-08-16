# Servlet

要想java类变成Servlet 必须符合一定规范：

​	① 必须继承	javax.servlet.http.HttpServlet

​	② 重写其中的 docGet() 或 doPost() 方法

docGet ()：接受 并处理 所有get 提交方式的请求

docPost ()：接受 并处理 所有post 提交方式的请求



Servlet 要想使用 必须配置

​	Servlet 2.5：web.xml

​	Servlet 3.0：@WebServlet

项目的根目录：WebContent   src

<a href ="welcomeServlet"> 所在的jsp是在 webContext 目录中，因此发出的请求WebcomeServlet 是去请求项目的根目录

Servlet 流程：

​	请求 --> <url - pattern> --> 根据<servlet-mapping>中的<servlet-name> 

​	去匹配 <servlet> 中的 <servlet-name> 然后找到<servlet-class>

​	最终将请求交给交给<servlet-class>执行



项目根目录：

​		WebContent . src (所有构建路径)

​	例如：WebContent 中 有一个index.jsp

​		src 中 有一个Servlet.java

​	如果：index .jsp 中请求 <a href="abc"></a> 则寻找范围：既会在src根目录中找，也会在WebContent 根目录找



/ 的区别：

​	web.xml 中的/ ：代表项目根路径

​		http://localhost:8888/ServletProject/

​	jsp中的/：服务器根路径

​		http://localhost:8888/

构建路径：webConent：根目录



Servlet 的生命周期（五阶段）

加载	初始化	服务	销毁	卸载

加载

初始化：init()		该方法会在Servlet被加载并实例化以后执行（初始化只执行一次）

服务：service() :	dopost()、doget()（请求几次执行几次）

销毁：destroy()	Servlet 被系统回收时执行（关闭tomcat服务器时）

卸载



注意：

​	若想开启tomcat就执行初始化(2.5Servlet)在web.xml添(Servlet标签中)

​			<load-on-startup>1</load-on-startup>标签中的1就是开启第一个初始化的，

​			若想开启第二个或..则设数字几就好了



Servlet API：

​	由俩个软件包组成：对应于Http协议的软件包，对应于除了Http协议以外的其他的软件包既ServletAPI可以适用于任何通信协议。

​	我们学习的servlet 是位于 javax.servlet.http包中的类和接口是基于http协议。

![Servlet继承图](iamges\Servlet继承图.png)



设置响应编码：

​	response.setContextType("text/html;charset=utf-8");

​	response.setCharacterEncoding("UTF-8");



