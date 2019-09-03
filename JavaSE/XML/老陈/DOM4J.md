## DOM4J

一款操作XML的jar(增删查改)

需求：

​	dom4j-1.6.1.jar



##### Document 创建方式

> 创建一个空Document对象 org.dom4j.Document

```java
Document document = DocumentHelper.createDocument();
```

> 从xml文件中读取数据构成一个Document对象 org.dom4j.Document

```java
Document document = new SAXReader().read("xml文件路径");
```











##### XML的格式化对象

org.dom4j.io.OutputFormat

> 构造方法

```java
OutputFormat();  //用空参就好了
```

> 常用方法

```java
//设置编码
setEncoding();
//设置是否换行
setIndent(boolean doIndent): true / false;
//设置是否tab[子父分明]
setNewlines(boolean newlines)：true / false;
```





##### document生成xml文件

XMLWriter

> 构造方法

```java
XMLWriter();
```

> 常用方法

```java
//writer(Document document);
```



### 小案例