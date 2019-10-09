# Tomcat

一款免费开源的服务器.







#### Tomcat 配置

① bin：一些可执行文件（startup、shutdown）启动/关闭..

② conf：一些配置信息（server.xml，web.xml）设置端口号，首启动网页..

③ lib：tomcat的一些jar包

④ logs：一些日志文件

⑤ temp：临时文件

⑥ webapps：项目目录

⑦ work：jsp 转 servlet （java）文件



#### 虚拟路径 与 虚拟主机

当我们平常开启Tomcat的时候默认只能执行webapps内的项目，因为webapps就是一个虚拟路径

要是我们不想执行tomcat中的webapps的项目（要想执行一个项目还得把项目拖到webapps中

，这显然不是我们想要的结果），可以自己定义虚拟路径

===========================================================================

##### 虚拟路径

> 第一种 (去conf中server文件</Host>属性之上定义属性为 )

```java
<Context docBase="项目文件夹路径" path="相对路径">
```

项目名为：testProject

若项目在："F:\doceProject\testProject" 则这就是绝对路径

相对路径："/testProject"  相对路径就 = 绝对路径

> 第二种 

```java
进入conf -> Catalina -> localhost 创建.xml文件 文件名为项目名,不对则报错，当然也可以写root，写root的话页面中可以省略项目名，直接输入项目中的网页名，但不建议这样做.
```



##### 虚拟主机

​	原本www.znsd.com是解析为156.146.285.142(比喻而已)，但是我们可以让本机解析我们定义好的地址，也就是说我们访问的网站只是内部做了一些处理将我们请求的网站转为了地址

​	第一步： conf -> server.xml

​		① 添加一个Host属性(Engine下的)

​			<Host name="地址" appBase="项目及地址">

​			<Context docBase="绝对路径" path="/">

​		② 将Engine内的默认Host 更改为定义好的域名/网址

​		③ 进入系统System32 -> drivers -> etc -> hosts

​			这文件内可以设置网址解析，本机的输入提交的会先到这里解析，

​			只要将需要解析的网址 与 对应的ip写入即可

​			如：127.0.0.1 www.baidu.com

​			若请求百度时则进入本机地址









#### 课程提前知

​	jsp底层就是一个servlet ，以前没有jsp的时候写的就是servlet但servlet写网页太麻烦了，所以诞生了jsp

jsp可以转为servlet，servlet 可以转为 jsp

当客户端对tomcat发请求时，tomcat响应并将该文件编译 成 java文件，然后运行 java 文件产生一个.class文件

所以若是大型网站编译会很慢（比较慢），但是只是针对第一次请求，出发用户对文件进行了修改，因为会再次

编译加载修改过的数据.























#### Eclipse 配置 tomcat 服务器

第一步：创建web项目

![1565353877763](images\Eclipse_01.png)

第二步：导入tomcat 包

![1565354179371](images\Eclipse_02.png)



第三步，一路Next，勾选web.xml

![1565354179371](images\Eclipse_03.png)



Finish 就ok了

该web.xml中 可以设置默认启动页，也就是说客户端输入网址进入的第一个界面











































#### Idea 配置 tomcat 服务器



第一步：创建Web 项目

![1565356302244](images\Idea_01.png)



第二步：配置tomcat 启动 项目

![1565356302244](images\Idea_02.png)

![1565356302244](images\Idea_03.png)![1565356302244](images\Idea_04.png)![1565356302244](images\Idea_05.png)



之后应用（apply）保存就ok了



若想选择用户默认进入页面则回到刚才的地方，将这里修改为想进入的页面就好了

![1565356302244](images\Idea_06.png)















#### 手动部署项目

首先将项目打包成war包【eclipse右击项目导出搜索war，选择导出路径Destination 就可以打成war包】，将war包放入tomcat中的webapp下启动tomcat就可以访问该项目了。







idea手动

![idea_war01](images\idea_war01.png)

![idea_war02](images\idea_war02.png)



1为war包路径

![idea_war03](images\idea_war03.png)





