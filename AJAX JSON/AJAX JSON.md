# AJAX

Asynchronous JavaScript and XML（异步的 JavaScript 和 XML )



**没使用AJAX时**

​	当资源产生新数据时，用户想查看某一块区域的资源状况，必须重新加载整个界面

**解决的问题**

​	当资源产生新数据时，用户无需重新加载整个界面，只需重新加载指定区域界面



> 实现AJAX前需了解

**XMLHttpRequest 对象** 

所有现代浏览器均支持 XMLHttpRequest 对象（IE5 和 IE6 使用 ActiveXObject）。

```javascript
XMLHttpRequest 用于在后台与服务器交换数据。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
```

**XMLHttpRequest 的三个属性**

```java
onreadystatechange	
	存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。
readyState	
	存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。
	0: 请求未初始化
	1: 服务器连接已建立
	2: 请求已接收
	3: 请求处理中
	4: 请求已完成，且响应已就绪
status	
	200: "OK"
	404: 未找到页面
```

**open(method,url,async)  建立连接**

```javascript
method：请求的类型；GET 或 POST
url：文件在服务器上的位置
async：true（异步）或 false（同步） 
```

  **send(String args)  发起请求**

```javascript
将请求发送到服务器，string：仅用于 POST 请求
```

**Get 还是 Post ？**

```javascript
与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。

	然而，在以下情况中，请使用 POST 请求：
		无法使用缓存文件（更新服务器上的文件或数据库）
		向服务器发送大量数据（POST 没有数据量限制）
		发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠
```

**onreadystatechange 回调函数**

```java
当readyState状态值到达2或超过2时会调用onreadystatechange一次，若onreadystatechange属性中不进行判断那么当执行ajax时存在onreadystatechange事件的话那么会被调用3次
```

**回调事件案例**

```javascript
//没判断的 ，该事件会触发3次 过程 2 3 4 状态时触发
ajax.onreadystatechange = function() {
	alert(http.responseText);
}
//进行判断的 请求已完成，且服务器执行完毕 才会执行 alert(http.responseText);
ajax.onreadystatechange = function() {
    if(ajax.readyState == 4){
       alert(http.responseText);
    }
}
```







## JavaScript 实现 AJAX

> 创建XMLHttpRequest对象

```txt
XMLHttpRequest为AJAX的核心，这个对象就可以实现AJAX
创建时需要考虑浏览器的类型，不同浏览器创建XMLHttpRequest方法不同
```

```javascript
// 创建XMLHttpRequest对象
function createXMLHttpRequest(){
    var xhr;
    //不同浏览器创建XMLHttpRequest方法不同。
    if(window.ActiveXObject){ //如果是IE浏览器
      xhr = new ActiveXObject("Microsoft.XMLHTTP");
    }else if(window.XMLHttpRequest){ //非IE浏览器
      xhr = new XMLHttpRequest();
    } 
    return xhr;
}
```

```javascript
//通过XMLHttpRequest对象开启AJAX
var ajax = createXMLHttpRequest();
//开启AJAX  请求方式：get  请求资源：AJAXServlet?参数名=参数值 是否开启异步：true
ajax.open("get","AJAXServlet?name=zhangsan",true);
//若为post则不能在请求资源中写参数，可以在send("name=zhangsan")写参数，但get不行
ajax.send();
//回调函数 
ajax.onreadystatechange = function() {
    if(ajax.readyState == 4){
		alert(http.responseText);
    }
}
```

















## Jquery 实现 AJAX

注意导入jquery

**第一种方式  $.ajax()**

```javascript
$.ajax(){
    url:"AJAXServlet",	//请求路径
    type:"POST", //请求方式
    data:{name:"张三",age:"18"}, //请求数据 or data:{"name=zhangsan&age=123"}
    success:function(data){	//请求成功时的回调函数 ajax.readyState == 4 data表示服务器响应的数据
        					//比如response.getWriter().write("你好"); data = "你好";
        alert("成功！"+data);
    },
    error:function(){ //当请求中发生异常则会执行的回调函数error
        alert("出错了噢！");
    }
    dataType:"text"	//当服务器响应时以什么方式接收参数
}
```

**第二种方式 $.get()**

```javascript
$.get(url,[args],[callback],[type]) //请求资源路径 发送请求参数 回调函数 响应时返回的类型
```

**案例**

```javascript
function ajax(){
    $.get("AJAXServlet",{name:"张三"},function(){alert("响应完毕")},"text")
}
```



**第三种方式 $.post()**

```javascript
$.post(url,[args],[callback],[type]) //请求资源路径 发送请求参数 回调函数 响应时返回的类型
```

**案例**

```javascript
function ajax(){
    $.post("AJAXServlet",{name:"张三"},function(){alert("响应完毕")},"text")
}
```

**注意重写Servlet的doGet 与 doPost 方法**



生成maven项目

```java
mvn archetype:generate -DgroupId=com.mycompany.app DartifactId=myapp 
-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 
```

将项目打成jar包只需要在pom.xml目录下输入命令

```java
mvn initiall
```

```xml
<mirrors>
<!--阿里代理镜像地址-->
	<mirror>
		<id>alimaven</id>
		<name>aliyun maven</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
		<mirror0f>*</mirror0f>
	</mirror>
</mirrors>
```













# JSON

JavaScript Object Notation  主要以键值对形式定义.

```java
"JavaScript 对象 表示法"
```



## **定义方式**

​	 **{键:值,键:值}**  "键"、'键'、键 没有区别，但定义键时建议统一格式，要么都""要么都什么都不带

**简单定义**

```javascript
var i = {userName:"张三",age:18,sex:"男"};
```

```java
//相当于 map.put("userName","张三").put("age",18)...
```



**复杂定义**

```javaScript
var i = {user:{userName:"张三",age:18,sex:"男"}};
//相当于 map.put("user",new User("张三",18,"男"));
```



**其他复杂定义**

```javascript
var i = [{user:{userName:"张三",age:18,sex:"男"}},{user:{userName:"李四",age:18,sex:"女"}}]
```

```javascript
var i = [{user:{userName:"张三",age:18,sex:"男"},{user:{userName:"张三",age:18,sex:"男"}}]
```

等一些复杂的格式







## JS获取json方式

**俩种方式：** json对象.键 、json对象["键"]



**案例演示**

```javascript
var i = {userName:"张三"}; // i 就是 json 对象
alert(i.userName);  //结果 张三
```

```javascript
var i = {userName:"张三"}  // i 就是 json 对象
alert(i["userName"]); //结果 张三
```

```javascript
var i = [{userName:"张三"}];
alert(i[0].userName); // 结果 张三
```



### 迭代Json

**for in**

```javascript
for(var key: i){
    alert("key="+key+"|value="+i[key]);
}
```

forin 会将json的key以字符串方式迭代出来，通过i[key]获取value，若通过i.key是迭代不出来的因为就相当于i."key"所以只能使用i[key]











### JavaBean 转 Json

```java
常见的解析器: Jsonlib, Gson, fastjson, jackson
```



**jackSon所需jar包**

![1](images\1.png)

**创建ObjectMapper对象**

```java
ObjectMapper oMapper = new ObjectMapper();
```

**将bean转为字符串并存入文件中 **

```java
writeValue(Object bean,File file): 有许多重载方法，括号内File 可以替换为 Output、writer
```

bean 为要被转为 Json 的 JavaBean 对象

**将bean转为字符串**

```java
String writeValueAsString(Object bean): 直接转为字符串返回
```

**案例演示**

```java
//导包
ObjectMapper oMapper = new ObjectMapper();
String json = oMapper.writeValueAsString(new User("张三",13));
System.out.println(json);
```

```java
class User{
	private String name;
    private Integer age;
    public User(String name,Integer age){
        this.name = name;
        this.age = age;
    }
}
```

**Map、List 转json**

```java
若传入的为Map则json的键为map的键，map键对应的值为json的值{user1:{},user2{}}
若传入的为List则转为的格式为数组中套着许多的json对象[{name:"张三",age:12},{name:"李四",age:16}]
```











**注解**

**@JsonIgnore: 排除属性**

```java
@JsonIgnore //被该注解注解过的属性不会出现在json中
```

```java
user{"张三","12"}:原本转json的数据为{name:"张三",age:"12"}
user{"张三","12"}:该对象的age被@JsonIgnore注解过之后转换的json为{name:"张三"}
```



**@JsonFormat: 属性的格式化**

```java
@JsonFormat(pattern = "yyyy-MM-dd") //主要格式化bean中带有date类型
```







**Json 转 JavaBean**

```java
readValue(String json,Class class); //json对象 {name:"张三"} User.class
```

