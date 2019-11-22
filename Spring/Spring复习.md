# Spring

```java
spring frameWork 它是一个开源的项目，而且目前非常活跃；它是一个基于IOC和AOP的构架多层j2ee系统的框架，但它不强迫你必须在每一层中必须使用Spring，因为它模块化的很好，允许你根据自己的需要选择使用它的某一个模块；它实现了很优雅的MVC，对不同的数据访问技术提供了统一的接口，采用IOC使得可以很容易的实现bean的装配，提供了简洁的AOP并据此实现Transaction Management，等等......
```







## 快速入门

使用maven 开发，pom.xml中引入依赖

```xml
<dependencies>
	<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
	<dependency>
		<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>5.1.5.RELEASE</version>
		</dependency>
</dependencies>
```

## 引入约束

maven的配置文件存放在resources下，web存放在src下.

spring的xml名字可以随意但一般名为applicationContext.xml

```xml
<!-- 引入约束 -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- 内容 -->
</beans>
```





## IOC

IOC 又称为 控制反转 意思就是说开始有一些权力是属于用户的而现在将这些权力转给spring容器，spring可以做得更好.



## XML配置

spring生成对象.

**bean内属性介绍**

```
https://blog.csdn.net/ZixiangLi/article/details/87937819
```

**属性类**

```java
package com.znsd;
public class User{
    private String admin;
    private String psWord;
}
```



**步骤：**

​	**\> **maven pom.xml 中引入依赖

​	**\>** 创建applicationContext.xml 引入约束

​	**\>** applicationContext.xml 中配置要创建的对象信息

以上俩步不演示了，直接跳到第三步

```xml
<!-- applicationContext.xml -->
<!-- 此处应有约束，但省略 -->
<beans>
    <bean name="user" class="com.znsd.User"/> <!-- name为获取class实例时的键 -->
</beans>
```

```text
在bean中 name 与 id 是一样的，以上也可以使用id="user"，但当需要给class多个其他名称则id是不行的
比如name="user1,user2,user3" 获取方式 "user1" 或 "user2" ... name使用,代表多个不同名称
id="user1,user2,user3" 获取方式 "user1,user2,user3" id使用,就代表字符串没有任何含义
```







### 第一种方式

```java
public void test(){
    ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
    User user = (User)ac.getBean("user");
}
```

当创建ApplicationContext首先开始解析xml，然后会将xml中的所有对象全部创建，当调用getBean时直接拿创建好的就可以了，默认饿汉单例，第二次getBean时还是同一个对象.



### 第二种方式

```java
public void test(){
    //全都是spring的类
    BeanFactory bf = new XMLBeanFactory(new Resoures("applicationContext.xml"));
    User user = (User)bf.getBean("user");
}
```

当创建BeanFactory对象时解析xml，当调用getBean时对象才会被创建，默认懒汉单例.





### 第三种方式

利用工厂创建对象。

**工厂类**

```java
public class UserFactory{
    public static User getUser(){
        return new User();
    }
}
```

工厂获取方式需要多加一些属性

```xml
<!-- applicationContext.xml -->
<!-- 此处应有约束，但省略 -->
<beans>
    <bean class="com.znsd.User" factory-method="getUser" name="user"/>
</beans>
```

```java
public void test(){
    ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
    User user = (User)ac.getBean("user");
}
```







## Java配置

**java配置方式**

```java
@Configuration	//声明该类为配置类
class AppliCation{
    @Bean()  // bean中不放值默认name为方法名，设置之后获取值为设置的值
    public User getUser(){
        return new User();
    }
}
```

**获取方式**

```java
public void test(){
    ApplicationContext ac = new AnnotationConfigApplicationContext(AppliCation.class);
    User user = (User)ac.getBean("getUser");
}
```

与xml不同的是使用的为另外一个类解析，类名AnnotationConfigApplicationContext，若想属性注入的时候直接在java配置类中点set方法就行😄/笑喷





















## 属性注入

在创建对象时注入一些数据到该对象内.



### 值注入

#### 第一种方式

构造注入，通过设置对应的值找到对应的构造方法.

```xml
<!-- constructor-arg 标签 -->
<bean name="user" class="User">
	<constructor-arg name="admin" value="张三"/>
	<constructor-arg name="psWord" value="123" />
</bean>
<!-- name为字段名 value为字段值 name也可以替换为index下标方式注入 -->
```

```xml
<bean name="user" class="User">
	<constructor-arg index="0" value="张三"/>
	<constructor-arg index="1" value="123" />
</bean>
```

以上找到的构造方法为俩个参数的.







#### 第二种方式

setter注入，通过调用该对象的set方法设置值

```xml
<bean name="user" class="User">
	<property name="admin" value="123"/>
	<property name="psWord" value="zhangsan"/>
</bean>
```







#### 第三种方式

p空间注入

```xml
xmlns:p="http://www.springframework.org/schema/p"
```

**案例演示**

```xml
<bean name="user" class="User" p:admin="张三" p:psWord="123"/>
```







### 引用注入

跟值注入区别不大.

**属性类**

```java
package com.znsd;
public class User{
    private Car car;
    private String[] arr;
    private List list;
    private Map map;
    private Properties pties;
    //此处应有构造 | setter 但省略
}
```

```java
package com.znsd;
public class Car{
    private String carName; //车名
    private String carAge; //车龄
    //此处应有构造 | setter 但省略
}
```





#### 普通对象注入

所有属性类在引用注入标题下

##### 第一种方式

将Car注入到User中

```xml
<bean class="com.znsd.Car" name="cars" />
<bean class="com.znsd.User" name="user">
    <property name="car" ref="cars"/> <!-- name="User的属性名称" ref="引用类型" -->
</bean>
```



##### 第二种方式

将Car注入到User中

```xml
<bean class="com.znsd.Car" name="cars" />
<bean class="com.znsd.User" name="user">
    <constructor-arg ref="cars"/> <!-- 一个参数的构造方法类型为Car -->
</bean>
```



##### 第三种方式

```xml

<bean class="com.znsd.User" name="user">
    <property name="cars">
    	<bean class="com.znsd.Car" name="cars" />
    </property>
</bean>
```



##### 第四种方式

p空间注入

```xml
xmlns:p="http://www.springframework.org/schema/p"
```

将Car注入到User中

```xml
<bean class="com.znsd.Car" name="cars" />
<bean name="user" class="User" p:cars-ref="cars"/>
```









#### 数组对象注入

所有属性类在引用注入标题下.

将数组注入到User中

```xml
<bean class="User" name="user">
	<property name="arr"> <!-- name="user类中的属性名称" -->
		<array>
			<value>1</value> <!-- 若为引用类型换引用类型的标签即可 -->
		</array>
	</property>
</bean>
```







#### List集合注入

```xml
<bean class="User" name="user">
	<property name="list">
		<list>
			<value>1</value> <!-- 若为引用类型换引用类型的标签即可 -->
		</list>
	</property>
</bean>
```







#### Map集合注入

```xml
<bean class="User" name="user">
	<property name="map">
		<map>
			<entry key="c1" value="张三"> <!-- 若为引用类型换引用类型的属性即可 -->
		</map>
	</property>
</bean>
```







#### Properties集合注入

```xml
<bean class="User" name="user">
	<property name="pties">
		<props>
			<prop key="uri">jdbc:mysql://localhost:3306/test_01</prop>
			<prop key="userName">root</prop>
			<prop key="passWord">123</prop>
		</props>
	</property>
</bean>
```







## Profile

根据用户不同的状态获取不同beans中的对象

#### xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <beans profile="xxx"> <!-- 当状态为xxx时 -->
        <bean class="User" name="user">
            <property name="admin" value="100"/>
            <property name="psWord" value="xxx" />
        </bean>
    </beans>
    <beans profile="ppp"> <!-- 当状态为ppp时 -->
        <bean class="User" name="user">
            <property name="admin" value="200"/>
            <property name="psWord" value="ppp" />
        </bean>
    </beans>
</beans>
```

**改变用户状态**

```java
public static void main(String[] args){
    String xmlPath = "ApplicationContext.xml";
    ClassPathXmlApplicationContext ac = new ClassPathXmlApplicationContext(xmlPath);
	ac.getEnvironment().setActiveProfiles("ppp"); //改变用户状态为ppp
	ac.refresh(); //刷新
	Object user = ac.getBean("user"); //get则获取的是xml中ppp状态的
	System.out.println(user); //结果{admin="200",psWord="ppp"}
}
```







#### 注解配置

```java
@Configuration //声明配置类
public class MyApplicationContext {

    @Bean("user")
    @Profile("xxx") //当状态为xxx时获取name为user时类型为User
    public User getUser(){
        return new User();
    }
    @Bean("user")
    @Profile("ppp") //当状态为xxx时获取name为user时类型为FactoryUser
    public FactoryUser getFactoryUser(){
        return new FactoryUser();
    }
}
```

**改变状态**

```java
public static void main(String[] args){
    AnnotationConfigApplicationContext acc = new AnnotationConfigApplicationContext();
	acc.getEnvironment().setActiveProfiles("xxx"); //更改配置为xxx
	acc.register(MyApplicationContext.class); //在此处注册配置类
	acc.refresh(); //刷新
	User bean = acc.getBean("user");
	System.out.println(bean.getClass());
}
```

















## 注解开发

使用注解代替xml配置

**引入约束**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
    ">
</beans>
```



### bean

开始使用bean标签配置对象，一旦对象多了则比较杂乱，现在使用注解代替大部分bean标签



**第一步**在xml还是需要配置一些标签，该标签主要作用是查找指定包下的bean类

```xml
<!-- applicationContext.xml -->
<beans> <!-- 约束在上方，此处省略 -->
    <context:component-scan base-package="com.znsd" ></context:component-scan>
</beans>
```

**第二步：**使用注解声明bean类 @Component("bean的name")

```java
package com.znsd
@Component("a") //若不写则name默认首字母小写
public class A {
    public void test(){
        System.out.println("我是a啊");
    }
}
//以上这种方式等同于 <bean class="com.znsd.A" name="a" />
```

**第三步：**获取该类实例

```java
public void test(){
    String xmlPath = "applicationContext.xml";
    ApplicationContext ac = ClassPathXmlApplicationContext(xmlPath);
    A a = (A)ac.getBean("a");
    a.test();
}
```

当创建application Context对象时首先解析传入的xml，然后当读取到context:component-scan标签时将base-package值内包下的类全部遍历一遍，找出被Component注解过的类，找到之后这些类都会被认为是bean对象，然后当用户通过applicationContext调用时若Component中没有值则将类名首字母小写作为name，否则就使用用户定义好的name.



### 声明bean类的四种情况

当不清楚该类是哪一层的时候则可以使用@Component() 若没值默认是类名首字母小写

```java
package com.znsd;
@Component("user")
class User{
    private String admin;
}
//等同于 xml中的<bean class="com.znsd.User" name="user" />
```

xml中需要配置路径，会扫描com.znsd下所有bean类

```xml
<!-- applicationContext.xml -->
<beans> <!-- 约束在上方，此处省略 -->
    <context:component-scan base-package="com.znsd" ></context:component-scan>
</beans>
```





若为控制层则使用 @Controller() 若没值默认是类名首字母小写

```java
package com.znsd;
@Controller("user")
class User{
    private String admin;
}
//等同于 xml中的<bean class="com.znsd.User" name="user" />
```

**xml中需要配置扫描标签注意**



若为业务逻辑层则使用 @Service() 若没值默认是类名首字母小写

```xml
package com.znsd;
@Service("user")
class User{
    private String admin;
}
//等同于 xml中的<bean class="com.znsd.User" name="user" />
```

**xml中需要配置扫描标签注意**



若为model层则使用 @Repository() 若没值默认是类名首字母小写

```java
package com.znsd;
@Repository("user")
class User{
    private String admin;
}
//等同于 xml中的<bean class="com.znsd.User" name="user" />
```



@Component @Controller @Service @Repository 他们功能相同都是用于声明bean类的，但含义不同注意.





### 属性注入

**案例演示**  目的：将B注入到A中

```java
@Component //声明bean类，若没指定名称则默认为类名首字母小写 @Component("abc") 获取时使用名称abc
public class A {
    //若没有指定名称则默认到spring中找对应的类型 @Resource("b")
    @Resource //自动注入，使用该标签将不需要set方法，直接去spring容器中寻找是否有B类型的Bean 
    private B b;

    @Override
    public String toString() {
        return "A{" +
                "b=" + b +
                '}';
    }
}
```

**B属性类**

```java
@Component //声明bean类，若没指定名称则默认为类名首字母小写 @Component("bbb") 获取时使用名称bbb
public class B {
    public B() {
        System.out.println("b跑起来了");
    }
}
```

**测试类**

```java
public class C {
    public static void main(String[] args) {
        ApplicationContext ac = new ClassPathXmlApplicationContext("ApplicationContext.xml");
        A a = ac.getBean("a");
        System.out.println(a);
    }
}
```

**使用到的注解**

```java
@Component: 当类被该注解声明后就相当于xml中的<bean class="该类全路径" name="注解中的名称">
```

```java
@Resource: 默认为属性的name，到spring中找是否有该name的bean，若指定则找指定name的bean.
```

```java
//也可以使用以下方式
@Autowired //当只有该注解时会在spring中找对应类型的bean
@Qualifier("car1")//搭配这种注解的时候会找对应类型以及名字为car1的bean
```









## 条件注解

根据不同的环境通过相同的方式获取不同的bean对象.

目录中的Profile就是一种体现方式.

```java
条件注解使用的注解为@Conditional(类.class) 实现条件注解必须搭配用户自定义的实现类实现Condition类
```

案例流程图**镇楼** 问？当获取name为show的bean时返回的是哪一个实例？

![条件注解案例01](images\条件注解案例01.png)

结果获取的为new User2()的实例

**案例**

**获取bean**

```java
public void test(){
    Class c = ApplicationContext.class;
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext();
    ac.getEnvironment().setActiveProfiles("window"); //设置用户环境
    ac.register(c);
    ac.refresh();
    MySystem bean = (MySystem)ac.getBean("show");
    bean.service(); //结果为"你所执行的方案为window..."
}
```

**Java配置类**

```java
@Configuration
class ApplicationContext{
    @bean("show")
    //WindowCondition.class该类的重写方法返回若为true则将该对象show:new WindowBean()装入spring容器
    @Conditional(WindowCondition.class)
    public WindowBean getWindowBean(){
        return new WindowBean();
    }
    
    @bean("show")
    //LinuxCondition.class该类的重写方法返回若为true则将该对象show:new LinuxBean()装入spring容器
    @Conditional(LinuxCondition.class) 
    public LinuxBean getLinuxBean(){
        return new LinuxBean();
    }
}
```

**用户实现类**

```java
class WindowCondition implements Condition{
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata TypeMetadata) {
        String[] profiles = conditionContext.getEnvironment().getActiveProfiles();
        for (String p : profiles) {
            if(p.equals("window")){
                return true;
            }
        }
        return false;
    }
}
class LinuxCondition implements Condition{
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata TypeMetadata) {
        String[] profiles = conditionContext.getEnvironment().getActiveProfiles();
        for (String p : profiles) {
            if(p.equals("linux")){
                return true;
            }
        }
        return false;
    }
}
```

**Java方案类**

```java
public interface SystemBean{
    void service();
}
public class WindowBean implements SystemBean{
    @Override
    public void service(){
        System.out.println("你所执行的方案为window...");
    }
}
public class LinuxBean implements SystemBean{
    @Override
    public void service(){
        System.out.println("你所执行的方案为Linux...");
    }
}
```













## 混合配置

Java配置引入xml配置

```xml
<!-- applicationContext.xml -->
<!-- 此处应有约束，但省略 -->
<beans>
    <bean name="user" class="com.znsd.User"/>
</beans>
```

**Java配置**

```java
@Configuration
@ImportResource("appliationContext.xml")
class ApplicationContext{
	//什么也不配置，但引入的xml中含有一个bean
}
```

**获取bean**

```java
public void test(){
    ApplicationContext ac = new AnnotationConfigApplicatonContext(ApplicationContext.class);
    User user = (User)ac.getBean("user");
    System.out.println(User);
}
```

