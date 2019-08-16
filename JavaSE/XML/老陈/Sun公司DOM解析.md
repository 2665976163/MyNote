# DOM解析

sun 内置的 一款解析方式

##### 一，动态创建XML 文件

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
        StreamResult streamResult = new StreamResult("C:\\Users\\Administrator\\Desktop\\TXT\\a.xml");
        transformer.transform(domSource,streamResult);
    }
```

当中的element 元素都是通过document对象创建的，element没有权力创建元素，只能追加元素

通过DOMSource 构造方法将节点对象转为数据源，利用变压器将数据源转为结果树，然后可以设置一下

显示的方式或者字符集编码等其他的，再创建结果集对象指定创建位置，然后用变压器将结果树写入

指定的结果集路径。





​	