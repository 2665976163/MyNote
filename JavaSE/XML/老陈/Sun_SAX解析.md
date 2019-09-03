# SAX 解析

SAX解析特点就是读一行解析一行，从上往下读，读一行释放一行，无法增删改，主要用于查询 .

想使用SAX解析首先通过SAXParser工厂创建 SAXParser 对象

```java
SAXParserFactory.newInstance().newSAXParser();
```

通过SAXParser 对象 中的 parse方法 将xml中的数据读取出来

```java
parse(File f, DefaultHandler dh) 
```

该方法需要俩个参数，一个xml文件路径，一个读取的格式对象.

sun只提供了样板[DefaultHandler] ，而内容得我们自己定义，所以需要继承DefaultHandler

```java
//创建我们的模板Handler 
class DIYHandler extends DefaultHandler{
    //重写方法：startDocument endDocument startElement endElement characters
    //startDocument/endDocument SAX开始解析调用的方法  可以做一些初始化/关闭资源操作
    //startElement/endElement SAX解析到标签执行的方法 可以对我们想要的标签做一些筛选
    //characters 若SAX解析到文本就执行该方法
}
```

以上重写的方法是我们可以用得到的方法

```java
public class SAXHandlers extends DefaultHandler {
    public static ArrayList<Hero> list;
    private String name;	//SAX解析若执行文本放则不清楚当前标签是什么所以定义变量保存当前标签
    private Hero hero;
    @Override
    public void startDocument() throws SAXException {
        System.out.println("开始解析===");
        list = new ArrayList <>();
    }

    @Override
    public void endDocument() throws SAXException {
        System.out.println("解析完毕===");
    }

    @Override
    public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
        if(qName.equals("Figure")){
            hero = new Hero();
        }
        name = qName;
    }

    @Override
    public void endElement(String uri, String localName, String qName) throws SAXException {
        if(qName.equals("Figure")){
            list.add(hero);
            hero = null;
        }
        name = null;
    }

    @Override
    public void characters(char[] ch, int start, int length) throws SAXException {
        String text = new String(ch,start,length);
        if(name != null){

            if(name.equals("name")){
                hero.setName(text);
            }else if(name.equals("age")){
                hero.setAge(text);
            }else if(name.equals("skin")){
                hero.setSkin(text);
            }
        }
    }
}
```

当我们调用parse 方法时将xml的file对象传入该方法，再创建以上模板的实例传给parse就好了.

> XML 内容

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Figures>
    <Figure id="1">
        <name>亚瑟</name>
        <age>男</age>
        <skin>狮心王</skin>
    </Figure>
    <Figure id="2">
        <name>项羽</name>
        <age>男</age>
        <skin>霸王别姬</skin>
    </Figure>
    <Figure id="3">
        <name>小乔</name>
        <age>女</age>
        <skin>天鹅之梦</skin>
    </Figure>
</Figures>
```

