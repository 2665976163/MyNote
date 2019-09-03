# OGNL表达式

**【目标】**

   	1. 了解OGNL的功能
   	2. 目前在哪里使用
   	3. 注意事项

## 【 入门 】

**【简介】**

```text
之前web阶段，学习过 【EL表达式】，【EL表达式】主要作用获取域对象中的值 .
OGNL 是一种功能更加强大的表达式，通过它简单一致的表达式语法，它使用相同的表达式去存取对象的属性
```

**【功能】**

 	1. **可以存取对象的任意属性**
		2. **调用对象的方法**
		3. **遍历整个对象的结构图**
		4. **实现字段类型转化等功能**

**【使用方向】**

 	1. 在 **【Struts2】** 中操作 值栈数据 .
		2. 一般把ONGL 在 Struts2中操作， 和【Struts2标签】一起使用操作值栈中的数据.

**【理清】**

​	OGNL 并不是 **【Struts2】** 的一部分，ONGL是一门单独的项目，只是经常与Struts2一起使用.

**【使用方式】**

 	1. 使用struts2 提供的OGNL jar包
		2. 在使用的jsp界面<%@ pace>下导入标签库

```html
<%@ taglib uri="/struts-tags" prefix="s" %>
```



**【温馨提示】**

1. 使用时注意是否配置了struts.xml【空的也行】
2. web.xml是否配置了过滤器【拷贝项目演示】



​	

**【使用小案例】**

**【目的】**

​	1.调用方法获取某字符串长度

注意温馨提示哦、、、、、、、、

> 案例

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="/struts-tags" prefix="s" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<s:property value="'啦啦啦啦'.length()" />
</body>
</html>
```

uri：标签库

prefix：别名

value中写ONGL表达式























## 【 值栈 】

**【简介】**

	1. 在web阶段时，使用**【servlet】**将数据存储到域对象中，使用**【EL】**将域对象中的数据取出.

```txt
【域对象】一定范围中的数据.
```

 	2. 在struts2中本身提供了一种机制，类似于域对象，也能将数据存取，它就是**【值栈】**
      	1. 在acton中存值，页面中取值



**【区别】**

​	**【Servlet】** 在第一次访问时被创建，之后访问使用的都是同一个，单例对象 。

​	**【Action】** 访问一次创建一次，非单例对象 。

```text
不信可以验证的.
```

**【值栈位置】**

​	每个访问产生的action中都有一个值栈对象（只有一个）

**【获取值栈对象】**

```java
ActionContext context = ActionContext.getContext();
ValueStack stack = context.getValueStack();
```

通过ActionContext获取到ValueStack对象，无论调用多少次，都获取的是同一个值栈 **.**



















### 【值栈内部结构】

#### **【简介】**

​	值栈内分为俩部分 root  与  context.

​	其中 root 继承了 ArrayList 默认内部有一个对自身action的引用.

​	context 实现了 map 接口 默认有一些固定的常见对象.

```txt
  Key			Value
request			request的引用
session			session的引用
application		ServletContext的引用
parameters		paramethers的引用
attr			attr的引用
```

**【注意】**

​	若三个域对象【request、session、servletContext】增加的name都一样，则使用context去获取域对象中的值，获取的是当中范围最小的value.













#### 【s:debug】

​	值栈内部比较复杂，若想看值栈内部结构比较困难，使用debug标签可以让开发人员

​	可视化观察内部存储结构.

​	**【注意】**

​		值栈是属于action，必须使用action跳转到jsp页面中，否则不是action的值栈信息.

​	**【使用位置】**

​		在jsp页面中使用，记得导入标签库，写struts配置文件、配置过滤器.

```html
<%@ taglib uri="/struts-tags" prefix="s" %><!-- 导入标签库，位置<%@ page 下 -->
<!-- 代码片段 -->
<body>
	<s:debug></s:debug>
</body>
```

**【使用debug的jsp界面图】**

![debug](C:\Users\Administrator\Desktop\JavaSE\struts2\images\debug.png)

点击超链接就可以观察值栈的内部存储数据了.







#### 【存放数据方式】

1. 通过调用值栈对象的set方法存放数据 .  

```java
public abstract void set(String key, Object o);
//因为是键值对，会让值栈产生一个map集合，而这些数据存放在map集合当中.
```

2. 通过调用值栈对象的push方法存放数据

```java
public void push(Object o);
//值栈产生一个对象
```

3. 在action中定义变量，提供get方法

```java
public class MyAction extends ActionSupport{
	private String str;
	
	public String getStr() {
		return str;
	}
    
	@Override
	public String execute() throws Exception {
		str = "数据";
		return "ok";
	}
}
```

以上说过，一个action有一个值栈，值栈内有对这个action的引用，若action中有非私有的数据，值栈都可以通过action的引用获取到想要的数据  。

**【重点】**

​	1. 通过set存值第一次会产生map对象，继续使用set会存储在第一次产生的map中【消耗一个位置】

​	2. 通过get存值，每一次使用都会在root中占用一个位置【零散，消耗大】

​	3. 通过action中定义变量，每次都存储在action中，【消耗一个位置】

**【注意】**

​	这三种方式真正存放的位置还是root中 .











#### **【开发注意】**

html注释时无法注释<> 

```html
<!-- <s:iterator> -->
```

要想在jsp中写以上这种情况

```html
<%-- <s:iterator> %>
```

则建议使用jsp内置注释.













#### 【取数据】

 	利用ognl表达式在jsp界面取出值栈数据 .

```java
class User{
    private String user;
    private String pswd;
    public String getUser(){
        return user;
    }
    public String getPswd(){
        return pswd;
    }
    public void setUser(String user){
        this.user = user;
    }
    public void setPswd(String pswd){
        this.pswd = pswd;
    }
}
```

**【演示案例 一】**

利用action在值栈中存单个值，通过ognl取单个值

```java
public class MyAction extends ActionSupport{
	private String str = "傻逼逼";	//action存储的数据
	
	public String getStr() {
		return str;
	}

	@Override
	public String execute() throws Exception {
		return "ok";
	}

}
```

> ognl  获取

```html
<s:property value="str"/>
```

**【演示案例 二】**

利用action在值栈中存对象，通过ognl取对象中的值

```java
public class MyAction extends ActionSupport{
	private User user = new User();	//action存储的数据
	
	public String getUser() {
		return user;
	}

	@Override
	public String execute() throws Exception {
        user.setUser("张三");
        user.setPswd("")
		return "ok";
	}
}
```

> ognl  获取

```html
<s:property value="user.user"/>
<s:property value="user.pswd"/>
```

**【过程】**

```text
通过user找到action中getUser，得到User对象，再通过pswd找到getPswd方法获取到pswd值.
```





**【演示案例 三】**

利用action在值栈中存List，通过ognl取出集合中的值.

```java
public class MyAction extends ActionSupport{
	private List list = new ArrayList();	//action存储的数据
	
	public String getStr() {
		return str;
	}

	@Override
	public String execute() throws Exception {
        list.add(new User)
		return "ok";
	}

}
```

- 第一种方式

```html
<s:property value="list[0].user"/>
<s:property value="list[0].pswd"/>
```

**【弊端】**

​	若不清楚集合中有多少数据还想遍历集合中全部数据则无法实现.

- 第二种方式

```html
<s:iterator value="list">
	<s:property value="user" />
    <s:property value="pswd" />
</s:iterator>
```

以上这种方式可以遍历list中全部数据.

- 第三种方式

```html
<s:iterator value="list" var="user">
    <!--
        遍历值栈list集合，得到每个user对象
        机制:把每次遍历出来的user对象放到context里面
        获取context里面数据特点:写ognl表达式，
        使用特殊符号 #
	-->
	<s:property value="#user.user" />
    <s:property value="#user.pswd" />
</s:iterator>
```

每次遍历出的对象都会临时存储在context中，而context是实现了map接口，所以是键值对存储的，以上var标签等于的就是context中的键，list遍历出的数据用var作为键存储在context中，所以var是可以随便定义的，不一定是user，也可以叫做abc，下面使用#abc.user也可以获取到属性值.

【其他方式】

​	getValueStack() 中 set、push 存数据 ongl 取数据 .

- set

```html
<s:property value="键" />	<!-- set(key,value); 用set的key就可以取值 -->
```

存的是对象的话

```html
<s:property value="键.属性名" />
```



- push

```html
<s:property value="[0].top" />  <!-- 代表取栈顶的第一个元素 0 1 2 3 .. -->
```

存的是对象的话

```html
<s:property value="[0].top.属性名">
```





## 【OGNL 表达式 # %的使用】

> 回顾

 ```txt
值栈内部分为俩块，root 与 context .
root继承arraylist	context实现map接口.
root 自带action的引用、context有request、response、servletContext等.
 ```

\#在OGNL表达式中用于表示获取Context中的内容.

**【案例演示 #】**

> action

```java
class MyAction extends ActionSupport{
    @Override
    public String execute(){
        HttpServletRequest hsr = ServletActionContext.getRequest();
        hsr.setAttribute("abc","666");
        return "ok";
    }
}
```

> jsp

```html
<s:property value="#request.abc" />	
```

以上先向域对象存储了一个值 666 键为 abc ，在jsp界面使用struts2 的标签 搭配ognl表达式获取到了context中的request对象然后将abc通过底层反射处理调用到了request的get方法获取到了abc的值 666; 结果:666



**【案例演示 %】**

​	如果直接在struts2 表单标签中使用ognl表达式struts2是不会识别的，只有加上%号才能识别.

```html
<s:textfield name="user" value="%{#request.user}"></s:textfield>
```

textfield为struts的表单标签.一般不会用，因为与普通的input区别不是很大，而且性能低.