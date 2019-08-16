# Javascript

简介：一门客户端脚本语言

​	> 运行在客户端浏览器中，每一个浏览器都有JavaScript解析引擎

​	> 脚本语言，不需要编译，直接可以被浏览器解析执行

功能：

​	可以用来增强用户和html页面交互的过程，可以控制html元素，让页面有一些动态效果

​	，让用户更好的体验。









## JavaScript 发展史

​	> 1992年，Nombase 公司，开发出第一门客户端脚本语言，专门用于表单校验

​	，命名为：C--  后来改名为：ScriptEase

​	> 1995年，Netscape（网景）公司，开发出了一门客户端脚本语言 LiveScript。

​	后来，请sun公司专家，修改liveScript，命名为JavaScript

​	> 1996年，微软抄袭JavaScript开发出了 JScript 语言.

​	> 1997年，ECMA（非洲计算机制造商协会），ECMAScript ，就是所有客户端脚本语言的标准

> JavaScript  =  ECMA + 网景开发的JavaScript自己的东西(BOM + DOM)











## ECMAScript 客户端语言标准

### 1.基本语法：



#### 1.**与html结合方式**

1. 内部 JS
   1. 定义：<script> 标签内就是js代码
2. 外部 JS
   1. 定义：<script src="">  标签中 src="外部js地址"

注意事项：

 1. script 可以定义在html中任意位置，但定义方式不同可能影响执行顺序.
 2. script 可以定义多个

#### 2.**注释**

1. // 注释内容
2. /* 注释内容*/

#### 3.**基本类型 与 引用类型**

##### 1. 基本类型

1. number 	整数/小数/NaN ( not a number 一个不是数字的数字类型(其他类型强转为number的值))
2. string  "a" || 'a' 都为字符串 ，js中没有字符的概念
3. boolean true/false
4. null  一个对象的空占位符
5. undefined 若一个属性未定义值则默认为该值

##### 2. 引用类型

​		对象

判断类型方法：typeOf( 参数 ) 返回真实类型

向网页写入数据：document.write(“内容”);

+(-)运算符

```java
document.write(+"abc");  //结果：NaN 在js中若运算数不为运算符要求的类型则会默认转换为该类型.
```

若转的类型为布尔则 false 为 0 || true为 1





#### 5. 对象

##### 1. Function 对象

> ##### 1.方式一
>

```javascript
var fun = new function(参数值,方法体);
如：
var fun = new function("a","b","alter(a);");
调用：fun(10,20);
```

> ##### 2.方式二
>

```javascript
function fun(a,b){
    alter(a);
}
调用：fun(10,20);
```

> ##### 3.方式三
>

```javascript
var test_1 = function fun(a,b){
    alter(a);
}
调用：test_1(10,20);
```

> 属性

```java
length（代表参数个数） test_1.lenth 就可以获取参数个数
```

> 特点

```java
1.方法定义时，形参类型可以不用写(var)
2.方法是一个对象，js中没有重载方法，方法名若相同则覆盖
3.在js中，方法调用只与方法名有关，与参数列表无关，不传或传超过指定参数都不会报错,正常运行
```





##### 2. Array 对象

> 定义方式 ( 三种 )
>

```javascript
//1. 创建一个空数组
var arr1 = new Array();
//2.自带三个初始化元素
var arr2 = new Array("abc",1,true);
//3.若参数只有一个且是数值类型则为数组的容量
var arr3 = new Array(20);
//以上三种可以被一种简写替代
var arr4 = [];
```

> 特点

```java
1.长度可变（类似java中的集合）
2.不存在下标越界，若下标大于数组容量则扩容.
```

> 方法

```java
参考w3c：https://www.w3school.com.cn/jsref/jsref_obj_array.asp
```





##### 3.Date 对象

> 定义方式

```javascript
var myDate=new Date()
```

> 特点

```java
获取时间，对时间做操作.
```

> 方法

```javascript
参考w3c：https://www.w3school.com.cn/jsref/jsref_obj_date.asp
```







##### 4.Math 对象

math 没有构造函数，当中方法全为静态，可以对象名调用

> 特点

```java
Math 对象用于执行数学任务。
```

> 方法

```txt
参考w3c：https://www.w3school.com.cn/jsref/jsref_obj_math.asp
```











##### 5. RegExp(正则) 对象

> 回顾正则表达式

```java
正则表达式
概念：用于定义字符串的组成规则.
	1.单个字符[]
    	如：[abc]、[a-z]、[a-zA-Z0-9]
    	\d：单个数组字符	[0-9]
    	\w：单个单词字符	[A-Za-z]
    2.量词符号：
    	?：代表0-1个
    	*：代表0或多个
    	+：代表1或多个
    	{m,n}：代表最低m 最多n {,n}0或n {m,}m或以上
    		贪婪模式：若满足m不会立即返回结果，会继续匹配，直到不满足才返回
```

> 正则对象 定义方式

```javascript
var reg = new RegExp(正则表达式);
var reg = /正则表达式/;
```

> ##### 常用方法

```java
boolean test(需要判断的参数);  //判断该参数是否符合正则表达式
其余相关内容参考w3c：https://www.w3school.com.cn/jsref/jsref_obj_regexp.asp
```









##### 6.Functions 对象

```javascript
该对象无需new ，该对象为全局对象，直接可以使用方法名
```

> 常用方法

```java
//1. 编码 解码  url无法解析中文所以需要将中文转义为二进制
encodeURI() decodeURI()
//2. 编码 解码
encodeURIComponent() decodeURIComponent()
//以上俩个的区别是第二个比第一个编码内容会多，连//都会编码，第一种较常用
//3. 解析一个字符串 并返回一个整数  只能解析数字开头的
parseInt()
//4. isNaN()
	判断是否为NaN，因为 NaN == NaN 都为false，所以需要这个方法判断是否为NaN
//5. eval()
	将 JavaScript 字符串，并把它作为脚本代码来执行。
其余全局方法参考 w3c:
	https://www.w3school.com.cn/jsref/jsref_obj_global.asp
```

























































### 2.BOM

全称：Brower Object Model	浏览器对象模型

主要内容：

​	Window、Navigator 、Screen、History 、Location

#### 1.Window 对象

1. Window 对象表示浏览器中打开的窗口。
2. 如果文档包含框架（frame 或 iframe 标签），浏览器会为 HTML 文档创建一个 window 对象，并为每个框架创建一个额外的 window 对象。
3. 注释：没有应用于 window 对象的公开标准，不过所有浏览器都支持该对象。

> 常用属性

```java
document：平常我们使用的document属性 是属于 window 对象的，平常没有察觉
```

无需创建的对象，使用方式有俩种：第一种对象.方法/属性，第二种可以直接方法/属性。

> 常用方法
>

```java
//1. 弹出窗口
alert()：显示带有一段消息和一个确认按钮的警告框.
confirm()：显示带有一段消息以及确认按钮和取消按钮的对话框. //返回 true 或 false
prompt()：显示可提示用户输入的对话框.
//2. 打开关闭浏览器
open(): 打开一个新的浏览器窗口或查找一个已命名的窗口 可以传指定地址.("http://baidu.com").
close(): 关闭当前窗口.
```

```txt
其余属性/方法 遵循w3c：https://www.w3school.com.cn/jsref/dom_obj_window.asp
```

> 定时器

```java
1.setTimeout() ：指定时间后执行参数内容.
  clearTimeout()  : 关闭定时器	
2.setInterval() ：每隔指定时间后执行参数内容.
  clearInterval()  : 关闭定时器
```

> 举例

```java
setTimeout("方法||代码",时间);
setInterval("方法||代码",时间);
以上俩个唯一区别：执行一次，执行无数次
```









#### 2. Location 对象

地址栏对象

> 属性 (主要对地址做操作 其他属性遵循 w3c )

```html
href	设置或返回完整的 URL。 //修改地址会自动刷新
```

> 方法

```html
assign()	加载新的文档。
reload()	重新加载当前文档。
replace()	用新的文档替换当前文档。
```

```txt
遵循 w3c ：https://www.w3school.com.cn/jsref/dom_obj_location.asp
```

































































































### 3.DOM

#### 1. Document 对象

> 常用方法

```javascript
getElementById("ID名");	//根据Id值获取该标签的对象
getElementsByTagName("标签值");	//根据标签获取所有相同的标签对象
getElementsByClassName("class名");	//根据class获取所有标签中与class相同的标签对象
getElementsByName("name名");	//根据name获取标签对象
craeteElement("标签名"); //创建一个元素
```

```javaScript
遵循w3c：https://www.w3school.com.cn/jsref/dom_obj_document.asp
```









#### 2.Element 对象

> 常用方法(一些对元素操作的方法)

```javascript
appendElementChild(元素对象);  //向当前标签追加一个子标签
setAttribute("属性名称","属性内容");  //向当前标签加入一个属性:如id = "id_1"
removeElementChild(子类对象);
遵循w3c：https://www.w3school.com.cn/jsref/dom_obj_all.asp
```







#### 3.HTML DOM 对象

html dom 对象分为三类：标签、DOM、样式

> 标签(a、img、body)等

```java
标签(用的时候查一下就可以了)遵循w3c：https://www.w3school.com.cn/jsref/dom_obj_anchor.asp
```

> DOM

```java
与JavaScript一致，document、element、attribute、even 等
```

> innerHTML 文本对象（属性设置或返回表格行的开始和结束标签之间的 HTML） 

```html
<!-- 替换操作 -->
element对象.innerHTML = "内容";
<!-- 追加操作 -->
element对象.innerHTML += "内容";
```















#### 4.事件

##### 1.点击事件

​	onclick：当用户点击某个对象时调用的事件句柄。 

​	ondblclick：当用户双击某个对象时调用的事件句柄。 





##### 2.焦点事件

​	onfocus  ：获取焦点

​	onblur  ：失去焦点





##### 3.加载事件

​	onload ：一张页面或一幅图像完成加载。 





##### 4.鼠标事件

onmousedown ： 鼠标按钮被按下。

onmousedown ： 鼠标按钮被按下。onmousemove  鼠标被移动。

onmouseout ： 鼠标从某元素移开。

onmouseout ： 鼠标从某元素移开。onmouseover  鼠标移到某元素之上。

onmouseup ：鼠标按键被松开。





##### 5.键盘事件

​	onkeydown ： 某个键盘按键被按下

​	onkeyup：某个键盘按键被松开

​	onkeypress：某个键盘按键被按下并松开。 





##### 6.选择与改变

​	onchange： 域的内容被改变。 

​	onselect：文本被选中。 





##### 7.表单事件

​	[onsubmit](https://www.w3school.com.cn/jsref/event_onsubmit.asp) ：确认按钮被点击。 

​	[onreset](https://www.w3school.com.cn/jsref/event_onreset.asp) ：重置按钮被点击。 



##### 其他事件

```java
遵循w3c：https://www.w3school.com.cn/jsref/dom_obj_event.asp
```























































































### 4.小案例

#### 1.点击开关

```html
<html>
  <head>
    <title>我的测试网页</title>
  </head>
  <style>
    #wrap_1{
      width: 100px;
      height: 100px;
      background: bisque;
    }
  </style>
  <body>
      
  <div id="wrap_1"></div>
  
   //js 逻辑
  <script>
    var wrap_1 = document.getElementById("wrap_1");
    var isOn = true;
    function onclick_1() {
      if(isOn){	//第一次点击 若条件满足 则修改指定内容 将条件改为false 用于开关
        wrap_1.style.width = "100px";
        wrap_1.style.height = "100px";
        wrap_1.style.backgroundColor = "#ff8c80";
        isOn = false;
      }else{	//第二次点击 条件已经为false 则进入以下属性 且将条件改为 true 
        wrap_1.style.width = "100px";
        wrap_1.style.height = "100px";
        wrap_1.style.backgroundColor = "#ff801d";
        isOn = true;
      }
    }
    wrap_1.onclick = onclick_1;
  </script>
  </body>
</html>
```

#### 2.轮播图

```html
<!doctype html>
<html lang="en">
 <head>
  <meta charset="UTF-8">
  <meta name="Generator" content="EditPlus®">
  <meta name="Author" content="">
  <meta name="Keywords" content="">
  <meta name="Description" content="">
  <title>Document</title>
 </head>
 <style>
	body{
		padding:0px;
		margin:0px;
	}
	#wrap_1{
		width:604px;
		height:298px;
	}
 </style>
 <body>
	<div id="wrap_1">
		<img id="image" src="images/1.jpeg" />
	</div>

	
 <script>
	var image = document.getElementById("image");
	var index = 1;
	alert(image);
	function carousel(){
		if(index > 5){
			index = 1;
		}
		if(index != 3){
			image.src = "images/"+index+".jpeg";	
			index++;
		}else{
			image.src = "images/"+index+".png";	
			index++;
		}
	}
	window.setInterval("carousel();",1000);
 </script>
 </body>
</html>
```

#### 3.页面自动跳转

```html
<!doctype html>
<html lang="en">
 <head>
  <title>切换地址栏</title>
 </head>
 <body>
	<input id="button_1" type="button" value="点击跳转"/>
	<script>
		var bean = document.getElementById("button_1");
		bean.onclick = skip;
		function skip(){
			open("index_2.html");
		}
	</script>
 </body>
</html>
```

```html
<!doctype html>
<html lang="en">
 <head>
  <title>Document</title>
 </head>
 <body>
	<span id="text"></span>
	<script>
		var bean = document.getElementById("text");
		var count = 5;
		bean.style.color = "#ff9900";
		function countDown(){
			bean.innerHTML = count--;
			if(count == 0){
				location.href = "http://www.baidu.com";
			}
		}
		setInterval("countDown();",1000);
	</script>
 </body>
</html>
```

#### 4.键盘输入获取键盘值

就是说wsad 为上下左右，我怎么让浏览器知道我按下了w或者s，就是依靠以下事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div id="wrap_1">
        <div id="">
            <input id="sss">
        </div>
    </div>
    <script>
        var root = document.getElementById("sss");
        root.onkeydown = function (ev) {
            alert(ev.keyCode);	//当我按下键盘触发onkeydown事件，引擎向方法传入一个ev对象
            				//ev.keyCode 代表的是该键的值.可以依靠这个值做操作
        };

    </script>
</body>
</html>
```

#### 5.表单提交是否允许提交

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div id="wrap_1">
        <div id="">
            <form action="#wasd" method="get" id="sss">
                <input type="submit">
            </form>
        </div>
    </div>
    <script>
        var root = document.getElementById("sss");
        var a = 0;
        root.onsubmit = function (ev) {
            var isTrue = false;
            if(isTrue || a>2){
                isTrue = true;
                alert("提交成功！");
            }else {
                alert("提交失败！");
                a++;
            }
            return isTrue;
        };
    </script>
</body>
</html>
```

form 标签中也可以写 onclick标签 然后专门写一个方法作为表单校验，该方法返回值为true / false

但切记，onclick内部是创建了一个临时的funcation 方法 调用我们的表单方法，虽然说我们定义的方法返回了

true/false 但它生成的不会返回，因为它内部是直接调用，没有return 所有我们需要在onclick 值中加入

："return 表单校验方法()"; 这样就可以了

> 内部调用

```javascript
function(){
    表单校验方法();
}
```

> 若在onclick中加了return，则为这样

```javascript
function(){
    return 表单校验方法();
}
```

这样也阻止表单提交



#### 6.表单校验

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div id="wrap_1">
        <div id="">
            <form action="#wasd" method="get" id="sss">
                user：<input id="user"/><span id="span_1"></span>
                passWord：<input id="psword"/><span id="span_2"></span>
                <input type="submit" />
            </form>
        </div>
    </div>
    <script>
        window.onload = function (ev) {
            var root = document.getElementById("sss");
            root.onsubmit = function (ev) {
                return user() && psWord();
            };
            document.getElementById("user").onblur = user;
            document.getElementById("psword").onblur = psWord;

        }
        function user() {
            var user = document.getElementById("user");
            var span_1 = document.getElementById("span_1");
            var isTrue = user.value;
            var reg = /[0-9]{5}/;
            var isCheck = reg.test(isTrue);
            if(isCheck){
                span_1.innerHTML = "√";
            }else{
                span_1.innerHTML = "×";
            }
            return isCheck;
        }
        function psWord() {
            var user = document.getElementById("psword");
            var span_1 = document.getElementById("span_2");
            var isTrue = user.value;
            var reg = /[0-9]{5}/;
            var isCheck = reg.test(isTrue);
            if(isCheck){
                span_1.innerHTML = "√";
            }else{
                span_1.innerHTML = "×";
            }
            return isCheck;
        }
    </script>
</body>
</html>
```

分析：

​	第一步	创建校验方法、

​		第二步 绑定离焦事件(当离焦之后自动调用校验方法然后进行判断)，

​		第三步 最后提交又调用一次校验判断是否为true.

​	第二步 与 第三步差不多 只不过一个是用于立即校验，一个用于表单提交