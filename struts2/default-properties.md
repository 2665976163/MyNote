# default.properties

以下为w3c 原文搬运的



声明：关于default.properties文件，本来是不计划讲的，毕竟不怎么使用。但是有朋友说有必要讲下，既然有人提，那我最好还是讲下，毕竟博文是面向初学者的，同时，是个人写博文，不掺有任何功利性，咱们也就随性写。只要有一人有需求，我就尽可能的满足大家。

 

一、学习案例：default.properties配置详解。

 

二、案例分析：讲解归讲解，但一些很鸡肋的，大家根本用不到的，我是不会讲的。初学者，学习新东西不是要大而全，而是适当最好。该记的记，该了解的了解。无关紧要的就随他去吧。开个玩笑。目的是不然大家迷糊和纠结一些没必要的东西，影响正常学习。

 

a)default.properties文件在struts2-core-2.3.15.3.jar中的org.apache.struts2目录下，大家可以在项目的引用包中打开。

 

b)struts.i18n.encoding=UTF-8
Struts2默认的编码类型是UTF-8。编码问题很恶心的，所以在编码统一时会使用。

 

c)struts.objectFactory = spring
Struts中action创建都是由对应的工厂创建。Struts自己提供了这样的一个工厂。当struts和spring进行整合后，这些action就交给spring进行打理。
后期框架整合时会用到。

 

d)struts.multipart.parser=jakarta
Struts的默认文件上传包，此处指定的是jakarta，即默认使用apache的fileupload组件。除非使用cos或pell才会修改。

 

e)struts.action.extension=action,,
表单提交或者url请求时地址的后缀。我们可以自行修改，但一般不用。没什么实际意义。项目真正上线，我们会对网站使用伪静态。以后的博文会讲。

 

f)struts.enable.DynamicMethodInvocation = false
动态方法调用。例如：http://....action!myMthod。但一般不会使用，因为它会暴露我们执行的方法名称，不太安全。

 

g)struts.devMode = false
开发模式。将一些警告信息转为错误信息，告诉开发者出现了什么样的问题。开始时改为true，发布时必须改为false。

 

h)struts.i18n.reload=false
对于开发来讲还是必将重要的。设置为true时，在每次请求时，资源包就会被重载。 resource bundles里面有个缓存，如果请求时，都会去缓存找，有则拿出来，而我们开发时，资源包是不断修改的，所以要禁止缓存避免影响开发调试。

 

i)struts.configuration.xml.reload=false
设置为true时，我们每次修改struts.xml文件后，框架会自动加载这个文件。

 

j)struts.ognl.allowStaticMethodAccess=false
页面使用静态方法。通过ognl标签调用值栈action中的方法。

 

k)Default.properties是不能直接修改的，我们如果要修改，有两种方式
1、在src下创建struts.properties
例如：struts.enable.DynamicMethodInvocation = true
2、在struts.xml中配置（推荐使用）
例如：<constant name=”struts.ognl.allowStaticMethodAccess” value=”true” />


三、经验之谈：
a)我们使用default.properties，一般也就使用以上几种，而且也不常用。了解下就行。
b)struts.devMode、struts.i18n.reload、 struts.configuration.xml.reload，我们开发时，最好配置上，有利于快速开发，当然我们也可以使用重启服务器解决，但毕竟项目在足够大时，重启服务器会很浪费时间的。说实话，我使用时，实用性没发现，反正每次还是重启服务器的。呵呵。大家可以自行测试。、