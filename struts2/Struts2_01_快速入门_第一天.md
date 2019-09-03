# Struts2

是吸收 struts 1 与 webwork 优点于一身的全新框架.

struts2 官网[阿帕奇的产品]地址

https://struts.apache.org/

## 回顾

```java
三层架构: web层  server层  dao层 组成.
    又称：视图层: 前台.(jsp..) 后台.(servlet..)  业务逻辑层:java  数据持久层:java.
```

而struts2 是用于代替web层 也就是视图层 的一款框架.



## jar 介绍

jar包 解压后的内容如下：

![struts 目录介绍](images\struts 目录介绍.png)

apps：功能演示【不会用就参考】

docs：文档

lib：jar包

src：源码





### 请求过程

![](images\请求过程图.png)

struts2 ：

当用户若配置了过滤器那么请求任意资源时首先该请求被过滤器截取，然后根据指定需求给予指定反馈.





### 小案例【快速入门】

**【注意事项】**

若出现**配置文件无法初始化**异常请仔细检查struts.xml单词是否打错



#### 1. 导入 jar 包

> 第一种方式

lib目录中有许多jar包【若不清楚导入哪些jar包参考第一个功能演示中的jar包】

**演示项目 ：**struts2-blank.war

**jar包路径：**webInfo/lib/*.jar 将这里面的所有jar全部拷贝到自己的项目中



#### 2. 创建 Action 类

前面说了，struts 2 是用于 代替web层的，那么之前我们写servlet时需要继承httpServlet，tomcat底层默认执行servlet中的server()方法，然后该方法被httpServlet实现了，主要功能将请求分散了.

若先使用struts 2也**需要一个类**似于servlet的类**叫做action** 当使用struts2时**默认执行**该类中的**execute()**方法

-----------------------------------------------------------------------------------------------------------------------------------------------------------

所以我们现在需要创建一个MyAction的类 写入一个叫做execute()的方法

![](images\Action_001.png)

创建完毕.



#### 3. 配置action 类访问路径

1. 创建 struts 2 核心配置文件

```txt
核心配置文件和位置是固定的，必须在src下面，名称为 struts.xml
```

2. 引入dtd 约束

到演示目录中找已经配置好的 struts.xml 文件复制该文件内的约束.

```dtd
<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
 "http://struts.apache.org/dtds/struts-2.3.dtd">
```

3. 一个简单配置

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!-- 此处有以上约束，但为了整洁暂且不加 -->
<struts>
    <package name="actionList" extends="struts-default" namespace="/">
        <!-- 用户可以通过地址栏输入name值访问到指定的action类 -->
        <action name="MyAction" class="action.MyAction">
            <!-- 若执行execute返回为ok则跳转到指定界面 -->
            <result name="ok">/index.jsp</result>
        </action>
    </package>
</struts>
```

 以上配置完毕，还需要再配置一个过滤器，因为当用户请求任何资源时，若我们不配置过滤器，那么请求将不会被分配给用户想访问的资源，就跟写了servlet不写配置文件一样，服务器并不清楚你是不是action，得通过过滤器识别.

#### 4. 配置过滤器

参照项目演示 /WEB-INF/web.xml

过滤器 struts2 已经提供好了，无需我们写，只需要我们配置，以下就是过滤器的配置信息

```xml
<filter>
   <filter-name>struts2</filter-name>
   <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
</filter>

<filter-mapping>
   <filter-name>struts2</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
```









### 核心配置文件

1. struts 文件位置固定[src/]
2. 设置配置文件主要的三个标签 package、action、result

#### package

类似于java中的包，主要就是将不同的action做区分，该标签中有以下这些属性

主要属性：name、extends、namespace

```xml
<package name="actionList" extends="struts-default" namespace="/">
	<action name="MyAction" class="action.MyAction">
       <result name="ok">/index.jsp</result>
    </action>
</package>
```

package中的 属性：

name：该包的名称，可以随便取，但尽量与功能相似

extends：继承 struts-default 意思就是说让该包中类具有action的功能，平时写servlet也需要继承httpservlet才有servlet的功能.

namespace：与包中的action name组成访问路径  namespace+action>name = /MyAction



#### action

```xml
<package name="actionList" extends="struts-default" namespace="/">
	<action name="MyAction" class="action.MyAction">
       <result name="ok">/index.jsp</result>
    </action>
</package>
```

action 中的 属性：

name：用户访问名称

class：action类路径

method：若不想执行execute方法则将想执行的指定方法写入.







#### result

```xml
<package name="actionList" extends="struts-default" namespace="/">
	<action name="MyAction" class="action.MyAction">
       <result name="ok">/index.jsp</result>
    </action>
</package>
```

result 中的 属性 ：

name ：根据action方法中的返回值，配置到指定路径中（可能是jsp、可能是action）

type：设置配置方式，请求转发或者重定向【默认请求转发】**【之后会有详细解释，先放一放】**











