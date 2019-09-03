# HTTP 

![HTTP请求](iamges\HTTP请求.png)

①是请求方法，GET和POST是最常见的HTTP方法，除此以外还包括DELETE、HEAD、OPTIONS、PUT、TRACE。不过，当前的大多数浏览器只支持GET和POST，Spring 3.0提供了一个HiddenHttpMethodFilter，允许你通过“method”的表单参数指定这些特殊的HTTP方法（实际上还是通过POST提交表单）。服务端配置了HiddenHttpMethodFilter后，Spring会根据method参数指定的值模拟出相应的HTTP方法，这样，就可以使用这些HTTP方法对处理方法进行映射了。   

②为请求对应的URL地址，它和报文头的Host属性组成完整的请求URL，

③是协议名称及版本号。   

④是HTTP的报文头，报文头包含若干个属性，格式为“属性名:属性值”，服务端据此获取客户端的信息。   

⑤是报文体，它将一个页面表单中的组件值通过param1=value1&param2=value2的键值对形式编码成一个格式化串，它承载多个请求参数的数据。不但报文体可以传递请求参数，请求URL也可以通过类似于“/chapter15/user.html? param1=value1&param2=value2”的方式传递请求参数。  





随便找一个浏览器举例一下，通过浏览器内置的抓包工具抓取网页的请求

![浏览器请求图](iamges\浏览器请求图.png)



> 详情参考

```java
https://www.cnblogs.com/xiaocao123/p/10392145.html
```



案例二

![浏览器请求图2](iamges\浏览器请求图2.png)





## Post 与 get 的区别

```java
Post ：
	1.数据是以流的方式写过去，不会在地址栏显示，现在提交到服务器使用的都是post
	2.流的方式写，所以数据没有大小限制
get ：
	1.会在地址栏后面后面拼接提交的数据，若为账号密码则会显示在地址栏，存在安全隐患，一般若提交的数据可以		公开的且不为文件则可以使用get.
    2.大小有限，1kb大小
```

