# Struts2

今天是 w 学 Struts2 的第二天，这是第二天的内容.



> 回顾

```xml
1.action 的三种创建方式、直接写execute方法、实现Action接口、继承Actionsupport类
2.根据用户请求的路径不同执行不同的方法【并非只执行execute】
	1. <action name="abc" class="action.abcAction" method="add"></action>
	2. <action name="abc_*" class="action.abcAction" method="{1}"></action>
	3. 动态执行对应方法【不清楚】
3.观察一些注意事项.
```





## 结果页面配置

当我们进行开发时难免会遇见方法返回值有些是一致的，但有些结果一致且跳转的地址也相同，那么就显得比较冗余.

> 举例

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
 "http://struts.apache.org/dtds/struts-2.3.dtd">
 <struts>
 	<package name="actionList" extends="struts-default" namespace="/">
        <action name="myAction_*" class="action.MyAction" method="{1}">
            <result name="跳转吧靓仔">/index.jsp</result>
        </action>
        <action name="YouAction_*" class="action.YouAction" method="{1}">
            <result name="跳转吧靓仔">/index.jsp</result>
        </action>
    </package>
</struts>
```

以上俩个action中除了是执行不同的action之外它们执行的方法都有可能返回"跳转吧靓仔"且跳转界面一致，那么若以后持续开发总会出现第三个第四个这样的，以上代码不会影响结果但出现过多冗余，那么可不可以把它们相同的result抽取出来，然后只要用户请求那么我就会到已有的result中找？ 可以**【全局 result】**

### 全局 result

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
 "http://struts.apache.org/dtds/struts-2.3.dtd">
 <struts>
 	<package name="actionList" extends="struts-default" namespace="/">
        <!-- 以下为全局结果页面配置 -->
        <global-result name="跳转吧靓仔">/index.jsp</global-result>
        <!-- 以上为全局结果页面配置 -->
        <action name="myAction_*" class="action.MyAction" method="{1}"></action>
        <action name="YouAction_*" class="action.YouAction" method="{1}"></action>
    </package>
</struts>
```

**【解析】**

当有用户请求时首先去action标签中通过反射执行class中的方法，然后获取到该方法的返回值，与action中的result标签中的name比较，若请求中的action没有result标签那么会去全局result找，若找到了则跳转指定路径，若action中有的话则优先执行action的result跳转指定路径【因为可能跳转的是另一个action】.



### 局部 result

以下仅为代码片段，action中的result就是局部result【平常使用的result就是局部result】

```xml
<action name="myAction_*" class="action.MyAction" method="{1}">
    <result name="跳转吧靓仔">/index.jsp</result>
</action>
```





### result type属性

> 回顾

```txt
请求转发：一次请求，一次响应，同一次请求参数还在，地址栏不会改变，只能请求项目中已存在的资源
重定向：俩次请求，俩次响应，参数不在了，地址栏会改变，可以请求网址资源
```



type中的值可以有 

【dispatcher】【redirect】：请求转发、重定向【用于跳转页面】

```xml
<!-- 片段代码 -->
<result name="跳转吧靓仔" type="redirect">/index.jsp</result>
```

【chain】【redirectAction】：请求转发，重定向【用于跳转action】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 此处应有约束但省略不写 -->
 <struts>
 	<package name="actionList" extends="struts-default" namespace="/">
        <action name="myAction" class="action.MyAction">
            <result name="ok" type="chain">YouAction</result>
        </action>
        <action name="YouAction" class="action.YouAction">
        	<result name="跳转吧靓仔">/index.jsp</result>
        </action>
    </package>
</struts>
```

当然也可以混淆使用，但是建议专业的人干专业的事.

【chain】不常用，会出现缓存问题.

【过程解析】

​	用户请求myAction，找到对应的action之后通过反射执行MyAction中的默认方法【里面可以做一些操作】，若结果返回"ok"则请求转发给



【练习题】

1.使用dispatcher 跳转一个界面  redirect 跳转一个界面

2.使用chain 跳转一个action redirectAction 跳转一个action











### Action类 获取提交表单数据

平常用户发起请求tomcat会将请求封装成对象传给servlet，然后servlet可以通request对象中的getParameter()

getParameterMap()获取到用户提交的数据，但Action中没有request对象。

**Action获取提表单参数方式  :**

1. ActionContext
2. ServletActionContext
3. 使用接口注入方式



#### ActionContext

1. 不是new出来的，虽然可以new但是需要提供参数内容【我们没有参数内容】
2. 通过调用该类中的静态方法创建ActionContext对象

> 获取请求参数方法

```java
public Object get(String key);
public Map<String,Object> getParameter();
```

> 案例

```java
public String execute() {
    //调用getContext获取ActionContext对象
	ActionContext context = ActionContext.getContext();
    //通过foreach将获取的参数遍历
	context.getParameters().forEach((k,v)->{
		System.out.println(k+"==="+((String[])v)[0]);
	});
	return "none";
}
```







#### ServletActionContext

> 静态方法【获取请求或响应对象】

```java
public static HttpServletRequest getRequest();
public static HttpServletResponse getResponse();
```

> 案例

```java
public String execute() {
	HttpServletRequest rquest = ServletActionContext.getRequest();
	rquest.getParameterMap().forEach((k,v)->{
		System.out.println(k+"===="+((String[])v)[0]);
	});
	rquest.getParameter("user");
	return "跳转吧靓仔";
}
```

##### 设置域对象

> 案例

```java
HttpServletRequest rquest = ServletActionContext.getRequest();
// 设置rquest域对象
rquest.setAttribute("name", "value");
HttpSession session = rquest.getSession();
// 设置session域对象
session.setAttribute("cookie", "cookieValue");
ServletContext context = ServletActionContext.getServletContext();
// 设置ServletContext 域对象
context.setAttribute("name", "value");
```

可以通过域对象传递参数.



















#### ServletRequestAware

实现该接口获取ServletRquest对象

**【补充】**

1. 实现ServletRequestAware获得request
2. 实现ServletResponseAware获得response
3. 实现ServletContextAware获得servletConntext

**【实现接口】**

```txt
在执行MyAction方法前struts2会根据实现的接口执行默认的拦截器创建request,response,aoolication对象 
```

> 案例

```java
public class MyAction implements ServletRequestAware{
	private ServletRequest context;

	@Override
	public void setServletRequest(HttpServletRequest request) {
		this.context = request;
	}
	public String execute() {
		ServletRequest rquest = context;
		rquest.getParameterMap().forEach((k,v)->{
			System.out.println(k+"===="+((String[])v)[0]);
		});
		rquest.getParameter("user");
		return "跳转吧靓仔";
	}
}
```







### 封装表单数据

将用户提交的数据封装到对象中.

#### 原始封装

1. 获取请求参数
2. 创建封装类对象
3. 将参数放入构造方法中

```java
public class MyAction{
    public String execute(){
        //用户提交的参数：user=123&password=666
        HttpServletRequest request = ServletActionContext.getRequest();
        String user = request.getParameter("user");
        String password = request.getParameter("password");
        User user = new User(user,password);
    }
}
class User{
    private String user;
    private String password;
    public User(String user,String password){
        this.user = user;
        this.password = password;
    }
    //此处应有get、set 省略不写
}
```







#### 属性封装

struts2 内置提供的自动封装方式

**【特征】**

​	属性必须在action类中【也就是说User类中的属性 get set方法全部移到MyAction中】

**【优点】**

​	无需任何操作，表单name名与action属性相同，拦截器会自动将收到的请求注入执行action中.

**【案例演示】**

```java
public class MyAction{
    private String user;
    private String password;
    public String execute(){
        //用户提交的参数：user=123&password=666
        HttpServletRequest request = ServletActionContext.getRequest();
        String user = request.getParameter("user");
        String password = request.getParameter("password");
        User user = new User(user,password);
    }
    public void setUser(String user){
        this.user = user;
    }
    public void setPassword(String password){
        this.password = password;
    }
    public String getUser(){
        return user;
    }
    public String getPassword(){
        return password;
    }
}
```

**【对比】**

​	**【原始封装】**

​	需要需要先获取request对象，通过request对象获取提交的表单值，然后new对象set将值注入.

​	**【属性封装】**

​	不需要任何操作，拦截器启动action方法之前会先看此action中是否有与表单的属性一致的若有则会将表单中的属性注入到action类中，然后再启动action类，此时action就可以使用当中的属性了

**【缺点】**

​	只能当前对象使用【注意看action的方法非静态的】，若想其他地方使用还是得 new对象，将本对象的值传入到new的对象值中.【可以传递this】





#### 模型驱动封装

struts2 内置提供的自动封装方式

**【特征】**

​	必须实现ModelDriven接口【泛型】指定要将属性封装的类

​	action类中必须有该封装类的实例【全局对象】.

**【优点】**

​	若要传递参数直接将该实例传递即可

**【案例演示】**

> Action 类

```java
public class MyAction implements ModelDriven<User>{
	private User user = new User();


	public String execute() {
		System.out.println(user);
		return "跳转吧靓仔";
	}
	@Override
	public User getModel() {
		return user;
	}
}
```

> 封装表单类

```java

class User{
	private String user;
	private String password;
	public String getUser() {
		return user;
	}
	public void setUser(String user) {
		this.user = user;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	@Override
	public String toString() {
		return "User [user=" + user + ", password=" + password + "]";
	}
	
}
```

**【对比】**

​	**【属性封装】**

​		将表单提交的数据注入到当前action对象，想传递要么this，要么new对象将当前数据放入对象中.

​	**【模型驱动封装】**

​		将表单提交的数据注入到【泛型】指定全局对象中，直接传递该全局对象.



**【使用属性封装与模型驱动封装注意问题】**

​	在一个action中获取表单提交的数据，可以使用属性封装也可以使用模型封装，但俩者不能同时获取

同一份表单的数据，若同时获取只有模型驱动可以获取到，而属性封装不能获取到。

【推敲】

​	因为当拦截器将获取到的请求想注入到action中，首先会判断action是否实现了modelDriven接口【泛型指定】，若实现了先从表单中找是否有该泛型指定的实例，若有就将表单数据通过反射拼接set方法注入到实例中，注入完毕之后就不会再判断该类中是否有属性了，那么就不会产生属性封装了.









#### 表达式封装

**【作用】**

​	可以将提交的表单数据封装到不同对象中

**【要求】**

1. 必须声明实体类
2. 必须生成get、set方法
3. 在表单中name中写表达式

**【表达式写法】**

​	 实例.属性名

**【案例演示】**

> 注意表单name

```html
<form action="bbb.action" method="get">
		<input type="text" name="user.userName" />
		<input type="password" name="user.password" />
		<input type="submit">
</form>
```

> action 类

```java
public class MyAction extends ActionSupport {
    //声明实体类
	private User user;
	//生成get、set方法
	public User getUser() {
		return user;
	}

	public void setUser(User user) {
		this.user = user;
	}

	public String execute() {
		System.out.println(user);
		return "跳转吧靓仔";
	}
}

class User {
	private String userName;
	private String password;

	public String getUserName() {
		return userName;
	}

	public void setUserName(String userName) {
		this.userName = userName;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	@Override
	public String toString() {
		return "User [user=" + userName + ", password=" + password + "]";
	}

}
```

**【对比】**

​	**【模型驱动封装】**

​		必须实现ModelDriven接口指定泛型，内部通过反射获取到action中指定泛型的已存在实例，拼接set方

​		法将属性注入，只能将表单数据注入一个实例中

​	**【表达式封装】**

​		拦截器校验name中的值是否在action有正确的实例.属性值，若有则到action中将请求的数据注入到不同指定实例中的属性中

> 片段

```html
<form action="bbb.action" method="get">
		<input type="text" name="user.userName" />
		<input type="password" name="person.password" />
		<input type="submit">
</form>
```

当拦截器拦截这条请求时，会将账号注入到Action类全局对象user中的userName属性中，密码注入到person中的password属性中，实现了让表单中的数据加入到指定的对象中.



**【思考】**

​	若真有一种场景需要同时将表单中的数据封装到不同对象中，虽然以上那种方式可以，但是action中的对象跟方法也会随之增多，那么可以使用集合【list、map】将它们装在一起，然后表达式就产生了一种新方式

举例：list{0}.user、list{1}.user  | map{"键"}.user

**【演示案例】**

```html
<form action="bbb.action" method="get">
		<input type="text" name="abc[0].user" />
		<input type="password" name="abc[0].password" />
		<input type="submit">
</form>
```

> Action 类

```java
class MyAction extends ActionSupport{
    List<User> abc;
    public List<User> getAbc(){
        return abc;
    }
    public String execute(){
        System.out.println(abc);
        return "none";
    }
}
```

使用abc的意思是想表示表单中的name不一定要用list，而是跟action中要封装的list名称有关系.





