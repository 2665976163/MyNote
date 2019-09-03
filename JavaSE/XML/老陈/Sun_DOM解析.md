# DOM解析

sun 内置的 一款解析方式

dom解析会将xml中标签首先分成Element[标签]，Attribute[标签内的属性]，Text[文本] 而Document就代表这三个，document也有一个父类称为Node，dom解析将整个xml中的数据一次性读到内存中形成一颗dom树，可以动态的做增删查改操作，因为每个节点都清楚自己的父节点是谁，可以在哪个元素增加子节点.

##### 一、动态创建XML 文件

```java
public static void createXml() throws Exception {
    //通过工厂创建Document对象
        Document document = DocumentBuilderFactory.newInstance().newDocumentBuilder().newDocument();
        Element figures = document.createElement("Figures");
        String[] name = {"亚瑟","项羽","小乔"};
        String[] skin = {"亚瑟王","霸王别姬","天鹅之梦"};
    	//利用for创建多个子元素
        for (int i = 0; i < 3 ; i++) {
            Element figure = document.createElement("Figure");

            Element nameElement = document.createElement("name");
            nameElement.appendChild(document.createTextNode(name[i]));

            Element age = document.createElement("age");
            nameElement.appendChild(document.createTextNode(i+1+""));

            Element skinElement = document.createElement("skin");
            nameElement.appendChild(document.createTextNode(skin[i]));

            figure.appendChild(nameElement);
            figure.appendChild(age);
            figure.appendChild(skinElement);

            figures.appendChild(figure);
        }
        //将节点对象放入DOM数据源
        DOMSource domSource = new DOMSource(figures);
        //变压器 主要将数据源转换为结果树
        Transformer transformer = TransformerFactory.newInstance().newTransformer();
        //设置 编码
        transformer.setOutputProperty(OutputKeys.ENCODING, "utf-8");
        //设置是否每个标签换行
        transformer.setOutputProperty(OutputKeys.INDENT, "yes");
    	//设置存储地址
        StreamResult streamResult = new StreamResult("C:\\Users\\Administrator\\Desktop\\TXT\\a.xml");
        transformer.transform(domSource,streamResult);
    }
```

当中的element 元素都是通过document对象创建的，element没有权力创建元素，只能追加元素

通过DOMSource 构造方法将节点对象转为数据源，利用变压器将数据源转为结果树，然后可以设置一下

显示的方式或者字符集编码等其他的，再创建结果集对象指定创建位置，然后用变压器将结果树写入

指定的结果集路径。

```java
DocumentBuilderFactory.newInstance().newDocumentBuilder().newDocument();
通过以上创建Document对象
```







##### 二、读取XML文件

```java
public static void main(String args[]){
    DocumentBuilder document = DocumentBuilderFacotry.newIstance().newDocumentBuilder();
    Document document = document.parse("xml路径");
    Element root = document.getDocumentElement();  //获取头节点beans
    NodeList list_Bean = root.getElementsByTagName("bean");	//获取一组bean
    for(int i=0;i<list.getLength();i++){
        Element root_bean = (Element)list.item(i);
        NodeList list_Name = root_bean.getElementsByTagName("name");	//获取一组name
        list_Name.item(0).getNodeValue(); //获取标签text文本值
        NodeList list_Age = root_bean.getElementsByTagName("age");	//获取一组age
        list_Age.item(0).getNodeValue(); //获取标签text文本值
    }
}
```







优点：

​	可以一次性将xml中的数据读取到内存中

缺点：

​	耗时.



​	