## SVN

一款代码托管软件

下载地址：<https://www.visualsvn.com/server/download/> 

安装教程：<https://jingyan.baidu.com/article/adc81513a6dfcbf723bf733c.html> 

![SVN目录](images\SVN目录.png)

**创建项目仓库**

```java
右击Repositories -> Create new Repository... -> 取个名称 然后一路next
```

**授权用户操作仓库权限**

```java
右击仓库properties -> add -> 将要授予权限的用户选中 -> [根据用户给与对应的权限] 然后确定
```









### eclipse SVN

一般来说eclipse中不是很高版本一般没有Svn插件，所以需要下载Svn插件

```java
http://subclipse.tigris.org/update_1.8.x
```

**eclipse配置教程**

```java
https://zhidao.baidu.com/question/1544313538169090187.html
```



#### 上传项目

右击项目 Team -> share Project...

![上传项目_01](images\上传项目_01.png)

选择SVN 下一步

![上传项目_02](images\上传项目_02.png)

使用已有资源库位置

![上传项目_03](images\上传项目_03.png)

看清楚选择，一个是用**项目名称**做上传的文件名称，一个是**自定义名称**作为上传的文件名称

![上传项目_04](images\上传项目_04.png)

上传项目的项目描述

![上传项目_05](images\上传项目_05.png)

然后点击Finish就将项目与SVN关联起来了，但若想将项目内容上传到SVN中则需要右击项目

![上传项目](images\上传项目.png)

Team -> 提交 就完成了











#### 拉取项目

![拉取项目_01](images\拉取项目_01.png)

然后Finish就ok了