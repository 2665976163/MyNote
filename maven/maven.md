# Maven

项目管理工具.



## 使用maven

将maven下载解压

```java
http://maven.apache.org/download.cgi
```

![maven目录](images\maven目录.png)

**配置环境变量**

```java
MAVEN_HOME:D:\软件\jar\maven\apache-maven-3.6.2
path:%MAVEN_HOME%\bin
```

**配置本地仓库**

conf/settings.xml 内

![maven_JAR](images\maven_JAR.png)

**配置阿里代理 **

若不配置也没事，可能下载会慢一些 conf/settings.xml 内

```xml
<mirrors>
<!--阿里代理镜像地址-->
	<mirror>
		<id>alimaven</id>
		<name>aliyun maven</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
		<mirrorOf>*</mirrorOf>
	</mirror>
</mirrors>
```

**生成maven项目 本地仓库**

```java
mvn archetype:generate -DgroupId=org.seckill -DartifactId=seckill -DarchetypeArtifactId=maven-archetype-webapp
```

将项目打成jar包只需要在项目的pom.xml目录下输入命令

```java
mvn initiall
```

**Settings.xml配置文件常用于配置本地仓库**





## eclipse配置Maven

window/preferences

**指定自己的maven，若没有可以使用eclipse默认的**

![eclipse指定自己maven](images\eclipse指定自己maven.png)

**配置settings配置文件路径，必须配置因为eclipse没有，可以网上复制粘贴**

![eclipse配置settings](images\eclipse配置settings.png)

**设置是否自动更新依赖，默认不自动更新，可以取消**

![eclipse自动更新jar](images\eclipse自动更新jar.png)









## 创建maven项目

**一、创建java项目**

File -> new -> Other -> 搜索Maven -> Maven Project

![Maven创建01](images\Maven创建01.png)

![Maven创建02](images\Maven创建02.png)





**二、创建web项目**

File -> new -> Other -> 搜索Maven -> Maven Project -> 创建简单的项目 -> 包名、项目名 将packging改为war

一个简单的web项目就创建完毕了

但一般来说这样创建时webapp下可能没有东西的

**解决办法**

右击maven项目 -> preferences -> Project Facets 

![Maven创建03](images\Maven创建03.png)

取消勾选，应用，然后再勾选，会出现蓝色的链接![Maven创建04](images\Maven创建04.png)

点击之后，更改生成路径，就解决报错了

![Maven创建05](images\Maven创建05.png)











## 配置JDK

**第一种方式 pom.xml 中配置**

```xml
<build>
	<plugins>
	<!--设置编译环境1.8 -->
		<plugin>
			<artifactId>maven- compiler-plugin</artifactId>
			<version>2.3.2</version>
			<configuration>
				<source>1.8</ source>
				<target>1.8</target>
			</configuration>
		</plugin>
	</plugins>
</build>
<!-- 使用该方式则每个maven项目都需要明确指定，并不能一劳永逸 -->
```

**第二种方式 settings.xml 中配置**

```xml
<profile>
	<id>jdk-1.8</id>
	<activation>
		<activeByDefault>true</activeByDefault>
		<jdk>1.8</jdk>
	</activation>
	<properties>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
		<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
	</properties>
</profile>
<!-- 一劳永逸，当创建maven项目时默认使用1.8的jdk -->
```





## 更新maven

修改了Maven配置文件需要更新maven.

右击maven项目/Maven/Update Project...  就ok了











## 打包

若是web项目则会在该项目中的target下生成该项目的war包，将该war包与普通web项目一模一样粘贴到webapps下，启动tomcat就可以访问该项目了

**右击 maven Web 项目**

Run As -> Maven install

然后一个war包就会生成在该项目的target目录下

找到该项目路径到target目录下复制粘贴即可







## maven 部署 tomcat

maven下的 pom.xml 中添加

```xml
<build>
	<plugins>
		<plugin>
			<groupId>org.apache.tomcat.maven</groupId>
			<artifactId>tomcat7-maven-plugin</artifactId>
			<version>2.2</version>
			<configuration>
				<port>80</port>
				<path>/</path>
				<uriEncoding>UTF-8</uriEncoding>
			</configuration>
		</plugin>
	</plugins>
</build>
```

右击Maven项目 -> Run As -> Maven Build ...

![tomcat配置01](images\tomcat配置01.png)

应用即可.若关闭再想启动点击该启动项即可

![tomcat配置02](images\tomcat配置02.png)

一般来说创建maven时都没有servlet的api,但发布时部署在tomcat会使用tomcat的，但若在编译期不想报错则在pom.xml中导入servlet的api

```xml
<dependencies>
	<!-- https://mvnrepository.com/artifact/javax.servlet/servlet-api -->
	<dependency>
		<groupId>javax.servlet</groupId>
		<artifactId>servlet-api</artifactId>
		<version>2.5</version>
		<scope>provided</scope> <!-- provided 代表该jar仅编译期存在，war包中不会存在 -->
	</dependency>
</dependencies>
```

但是导入该servlet api仅用于编译时防止eclipse报错，因为当maven发布时会将war包部署到tomcat内，tomcat内自带servlet api，所以不需要再添加该api，所以需要<scope>provided</scope>标签，防止该jar成为war包的一部分，若成为一部分则tomcat中就会存在俩个servlet api tomcat不清楚到底选哪个则报错，其他属性参考scope







### Scope详解

![scope](images\scope.png)









## 依赖冲突

当使用久了maven会发现导入一个jar包会将该jar包的依赖一同导入，当导入俩个jar包的依赖相同版本不同时的选择

**假设图**

![依赖冲突01](images\依赖冲突01.png)

**一、调换位置**

 ```xml
<dependencies>
    <!-- 假设有该jar -->
	<dependency>
		<groupId>com.xxx</groupId>
		<artifactId>struts2</artifactId>
		<version>1.0</version>
	</dependency>
	<dependency>
		<groupId>com.xxx</groupId>
		<artifactId>hibernate</artifactId>
		<version>1.0</version>
	</dependency>
</dependencies>
 ```

当前struts2在上hibernate在下，使用的xxx.jar版本为1.0，若想使用xxx.jar 2.0则将hibernate的位置移动到上方.



**二、直接选择目标依赖**

![依赖冲突02](images\依赖冲突02.png)

则会直接使用导入的，不会再去找导入jar的其他该依赖版本.





## 排除依赖

当导入某个jar时，不想加载该jar的某个依赖，则可以进行排除依赖

![依赖冲突03](images\依赖冲突03.png)

![依赖冲突04](images\依赖冲突04.png)

点击该处，然后会弹出一个框，点击ok即可

pom.xml中c3p0的依赖注入中会多一些东西，就是以下这些

![依赖冲突05](images\依赖冲突05.png)

这样就排除了c3p0中mchange的jar依赖，当导入c3p0时不会将该依赖一起导入.











## 统一版本号

当需要批量进行版本号管理时可以使用的标签.

```xml
<properties>
	<spring.version>4.3.21. RELEASE</spring.version>
</properties>
```

当导入其他maven jar配置时jar版本指向该版本即可

```xml
<!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
<dependency>
    <groupId>com.mchange</groupId>
    <artifactId>c3p0</artifactId>
    <version>${spring.version}</version> <!-- 这样 -->
</dependency>
```

