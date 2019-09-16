### MVC 模式

M：model 模型

V：View 视图

C：Controller 控制器



Model：一个功能，用javaBean实现

View：用于展示，以及用户的交互，使用html css js jsp 等前端技术实现

Controller：接收请求，将请求跳转到模型进行处理模型处理完毕后再将处理结果返回到请求处

，jsp可以实现（不推荐），推荐servlet 实现控制器.

​	JSP --> java(Servlet) --> JSP







### 三层架构

​	与MVC模式的目的一致：都是为了解耦和，提高代码复用

​	区别：二者对项目的理解角度不同

#### 三层组成：

​	表示层（USL User show layer：视图层）

​		--前台：对应 MVC 中的 view，用于和用户交互，界面显示

​		--后台：对应 MVC 中的 Contraller，用于控制跳转，调用业务逻辑层

​	前台：html、css、jsp、js、jquery等 后台：Servlet （SpringMVC Struts2）

​	业务逻辑层（BLL Business Logic Layer：Service层）

​	持久层	直接操作数据库，一些原子操作.

三层架构：
	三层架构是由 View [视图]  Server [服务]  Dao [持久] 组成
	

	View 视图层：
		 主要编写用户请求(html 、css 、js 、jsp 、jquery等)与对应的后台处理
	Service 服务层：
		主要处理用户请求的后台处理，将Dao 持久层 中的数据组成与之对应的处理方法
	Dao 持久层：
		直接操作数据库，一些原子操作.