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

##### 1. Fucnation 对象

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













### 3.DOM





