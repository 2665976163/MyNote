# Jsoup

jsoup 是一款Java 的HTML解析器，可直接解析某个URL地址、HTML文本内容。它提供了一套非常省力的API，可通过DOM，CSS以及类似于jQuery的操作方法来取出和操作数据。 



Jsoup开发需求：

​	jsoup-1.12.1.jar 



> Jsoup包内主要了解类

```java
Jsoup. (用于获取Document对象)
、Document. (用于获取Attribute、Element)
、Attribute. (属性)
、Element. (元素)
```



##### Jsoup类中 常用方法 

```java
 该类提供了很多重载方法，以下为常用的重载方法
 public static Document parse(File in, String charsetName)
 	参数1：解析的文件对象  参数2：字符集
 public static Document parse(String html)
     参数1：字符串类型的值(用字符串写的html)
 public static Document parse(URL url, int timeoutMillis)
     参数1：URL同一资源定位符	参数2：等待时间
```

> 举例 一（解析文件）

```java
public static void main(String[] args){
    String str = Main.class.getClassLoader().getResource("xml路径").getPath(); //要解析的xml路径
    Document document = Jsoup.parse(new File(str),"UTF-8");
    System.out.println(document);
}
```

> 举例二（解析字符串）

```java
public static void main(String[] args){
        String html = "<html>\n" +
                "\n" +
                	"<head>\n" +
                		"<title>我的第一个 HTML 页面</title>\n" +
                	"</head>\n" +
                	"\n" +
                	"<body>\n" +
                        "<p>body 元素的内容会显示在浏览器中。</p>\n" +
                        "<p>title 元素的内容会显示在浏览器的标题栏中。</p>\n" +
                	"</body>\n" +
                "\n" +
                "</html>";
        Document document = Jsoup.parse(html);
        System.out.println(document);
    }
```

> 举例三（解析网页）

```java
public static void main(String[] args) throws IOException {
        URL url = new URL("https://baike.baidu.com/item/jsoup"); //放入网址
        Document document = Jsoup.parse(url,10000);
        System.out.println(document);
    }
```

以上三种都可以获取Document对象

































本笔记中使用的xml路径文件内容如下

> xml中的文件内容

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans>
    <bean name="abc" class="bca">
        <property type="三四"></property>
    </bean>
    <bean name="abc" class="bca" id="123">
        <property type="三四"></property>
    </bean>
    <bean name="abc" class="bca">
        <property type="李四" id="222"></property>
    </bean>
</beans>
```

##### Document类中 常用方法

```java
public Element getElementById(String id)：//根据id值获取id所在的元素对象

public Elements getElementsByTag(String tagName)：//根据标签名获取对应的元素集合
    
public Elements getElementsByAttribute(String key)：//根据属性名称获取元素集合

public Elements getElementsByAttribute(String key,String value)：//根据属性名，属性值获取对应的集合

public Elements select(String cssQuery)：//选择器
```

> 举例一

```java
public static void main(String[] args) throws IOException {
        String str = Main.class.getClassLoader().getResource("xml路径").getPath();
        Document dc = Jsoup.parse(new File(str),"utf-8");
        Element elements = dc.getElementById("222");
        System.out.println(elements);
}	//获取到了元素中id值为222的元素对象
```

> 举例二

```java
public static void main(String[] args) throws IOException {
        String str = Main.class.getClassLoader().getResource("xml路径").getPath();
        Document dc = Jsoup.parse(new File(str),"utf-8");
        Elements elements = dc.getElementsByTag("bean");
        System.out.println(elements);
}	//获取到了标签为bean的元素对象
```

> 举例三

```java
public static void main(String[] args) throws IOException {
        String str = Main.class.getClassLoader().getResource("xml路径").getPath();
        Document dc = Jsoup.parse(new File(str),"utf-8");
        Elements elements = dc.getElementsByAttribute("id");
        System.out.println(elements);
}	//获取到了标签中带有属性名为id的元素对象
```

> 举例四

```java
public static void main(String[] args) throws IOException {
        String str = Main.class.getClassLoader().getResource("xml路径").getPath();
        Document dc = Jsoup.parse(new File(str),"utf-8");
        Elements elements = dc.getElementsByAttributeValue("id","123");
        System.out.println(elements);
}	//获取到了属性为id值为123的元素对象
```

> 举例五（Select选择器提供了很大的方便，可以迅速准确的查找需要的元素对象）

```java
public static void main(String[] args) throws IOException {
        String str = Main.class.getClassLoader().getResource("applicationContext.xml").getPath();
        Document dc = Jsoup.parse(new File(str),"utf-8");
        System.out.println(dc.select("bean > property[type=\"李四\"]"));
}//以上通过一行代码准确的找到了属性为李四的元素对象
```

解析：

​	> ：该标签的子标签

​	[] ：属性选择器









































##### Element类中 常用方法

```java
public Attributes attributes() //获取当前标签中的全部属性

public String text()  //获取当前标签或子集中的纯文本

public String html()  //获取当前标签或子集中的文本，以及标签
    
public String attr(String attributeKey) //根据属性名获取当前标签的属性值
```





















# XPath

快速查询有俩种方式一种是上面的select 另外一种就是XPath

**XPath**即为[XML](https://baike.baidu.com/item/XML)路径语言（XML Path Language），它是一种用来确定XML文档中某部分位置的语言。 

需要导入的jar包：

​	JsoupXpath-0.3.2、commons-lang3-3.4



需要了解的类有：

​	JXDocument



##### JXDocument类中	常用方法

```java
public List<Object> sel(String xpath)：//通过传入的标签名称获取标签object对象
    
public List<JXNode> selN(String xpath)：//通过传入的标签名称获取属性(JXNode内部是Element)对象
```

> 举例一

```java
public static void main(String[] args) throws IOException, XpathSyntaxErrorException {
        String str = Main.class.getClassLoader().getResource("xml路径").getPath();
        Document dc = Jsoup.parse(new File(str),"utf-8");
        JXDocument jxDocument = new JXDocument(dc);
        List<Object> jxNode = jxDocument.sel("//bean/property"); //---
        System.out.println(jxNode.get(2));
}	//该方法因为是获取的为Object所以无法获取指定标签内容，若想获取则强转
```

> 如

```java
public static void main(String[] args) throws IOException, XpathSyntaxErrorException {
        String str = Main.class.getClassLoader().getResource("xml路径").getPath();
        Document dc = Jsoup.parse(new File(str),"utf-8");
        JXDocument jxDocument = new JXDocument(dc);
        List<Object> jxNode = jxDocument.sel("//bean/property");
        Element element = (Element) jxNode.get(2);	//强转
        System.out.println(element.attr("id"));
}	//该方法获取到了当前标签 属性为id的值
```

> 举例二

```java
public static void main(String[] args) throws IOException, XpathSyntaxErrorException {
        String str = Main.class.getClassLoader().getResource("applicationContext.xml").getPath();
        Document dc = Jsoup.parse(new File(str),"utf-8");
        JXDocument jxDocument = new JXDocument(dc);
        List<JXNode> jxNode = jxDocument.selN("//bean/property[@id='222']");
        System.out.println(jxNode.get(0).getElement().attr("id"));
    }	//该方法获取到了当前标签 属性为id的值
```

解析：

​	//：为Document全部标签，就是说只要Docuement中出现了bean就可以找到

​	 /：代表该标签的子标签

​	[]：代表该标签中的元素