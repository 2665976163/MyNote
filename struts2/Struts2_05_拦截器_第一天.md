# Struts2 拦截器

**【简介】**

​	Struts2 是吸取 struts 与 webwork 优点于一身的全新框架 ，struts2中封装了许多功能，其实这些封装功能都封装在拦截器中。

​	Struts2 中有很多拦截器，但每次执行并不是执行所有的拦截器，而是执行一些默认的拦截器。

​	Struts2 默认拦截器的位置

​		**struts2-core-2.3.34.jar/struts-default**

​		**【拦截器】**

​	![拦截器](images\拦截器.png)

​		**【默认拦截器】**

​		![默认拦截器](images\默认拦截器.png)



拦截器在什么时候执行？

​	在action创建之后，方法执行之前.



## 拦截器的底层原理

拦截器使用俩个原理

【aop 面向切面】

​	aop是面向切面（方面）编程，有基本功能，扩展功能，不通过修改源代码方式扩展功能.

【责任链模式】

​	用户发起请求，请求被拦截器拦截，若条件满足则返回否则交给下一个拦截器进行处理，最后执行action方法



**aop思想 与 责任链  如何应用于拦截器中？**

​	**\>**  在action方法执行之前执行默认拦截器，执行过程使用 aop 思想，在action没有直接调用拦截器的方法，使用配置文件进行操作。

​	**\>** 在执行拦截器的时候，执行很多的拦截器，这个过程就是责任链模式



**过滤器 与 拦截器 的区别**

**【过滤器】**

​	过滤器可以过滤任何内容，比如：jsp、图片路径、servlet、html等

**【拦截器】**

​	拦截器只能拦截action。









## 自定义拦截器

- 第一种方式

  继承**【AbstractInterceptor】**抽象类 ，重写intercept方法.

- 第二种方式

  继承 **【MethodFilterInterceptor】** 类，重写doIntercept方法.

  **\>** 可以自由选择是否执行拦截器 .

**【案例演示】**

​	**【需求】**

​		**\>** 用户进行登陆时需要验证登陆密码，但action之前没有些验证功能，我又不想改源代码，此时验证			

​		功能就可以交给我们自定义拦截器，将用户的请求拦截进行验证，若ok则继续执行action中原本的功能，否则返回登陆界面【并不合理，只是想体现拦截器拦截请求而已，因为用户执行的每次操作都不一定需要验证登陆】.【本章案例】

​		**\>** 用户进行每次操作都需要进行验证是否在登陆状态，若不为登陆状态则跳转到登陆状态界面【合理，每次用户操作都可以验证是否为登陆状态，但是需要一些操作，临时案例，所以不选择使用】.

​	**【步骤】**	

​		**\>** 继承 MethodFilterInterceptor 重写doIntercept 方法

​		**\>** 获取用户提交的数据，进行校验，若正确则返回invocation.invoke()，否则跳转登陆界面.

​		**\>** 以上只是一个独自的自定义拦截器，然后将自定义拦截器与action产生关系

​			**\>** 声明自定义拦截器、使用自定义拦截器，使用默认拦截器栈【内有一些默认执行的拦截器】

> jsp

```html
<form action="myAction.action" method="get">
		账号：<input name="user" />
		密码：<input name="pswd" />
		<input type="submit" />
</form>
```

> 自定义拦截器

```java
public class MyInterceptor extends MethodFilterInterceptor {
	@Override
	protected String doIntercept(ActionInvocation invocation) throws Exception {
		HttpServletRequest hr = ServletActionContext.getRequest();
		String user = hr.getParameter("user");
		String pswd = hr.getParameter("pswd");
		System.out.println("执行了");
		if(user.equals("admin") && pswd.equals("123")) {
			return invocation.invoke();
		}else {
			return "login";
		}
	}
}
```

> action

```java
public class MyAction extends ActionSupport {
	@Override
	public String execute() throws Exception {
		return "ok";
	}
}
```

> struts.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
	"http://struts.apache.org/dtds/struts-2.3.dtd">
<struts>
	<package name="test_pack" extends="struts-default" namespace="/">
		<interceptors>
			<!-- 声明自定义拦截器 name：任意	class：自定义拦截器包名+类名-->
			<interceptor name="myInterceptor" class="action.MyInterceptor"></interceptor>
		</interceptors>
		<action name="myAction" class="action.MyAction">
			<!-- 使用自定义拦截器	name： 声明自定义拦截器的name名称-->
			<interceptor-ref name="myInterceptor">
				<!-- 这里可以添加不执行拦截器的方法  单个可以不使用,号分割 格式如下 -->
				<!-- <param name="excludeMethods">忽略的方法名,忽略的方法名...</param> -->
			</interceptor-ref>
			<!-- 
                若使用自定义拦截器若不配置默认执行的拦截器则只会执行自定义拦截器 
                以下为配置默认拦截器
 			-->
			<interceptor-ref name="defaultStack">
			</interceptor-ref>
			<result name="ok">/index.jsp</result>
			<result name="login">/login.jsp</result>
		</action>
	</package>
</struts>
```

