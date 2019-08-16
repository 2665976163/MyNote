## XML 解析

### DocumentBuilder

```java
使用这个类，应用程序员可以从XML获得一个Document 。 
这个类的一个实例可以从DocumentBuilderFactory.newDocumentBuilder()方法获得。 一旦获得此类的实例，可以从各种输入源解析XML。 这些输入源是InputStreams，Files，URL和SAX InputSources。 
```

> 常用方法

```java
public Document parse(File f)throws SAXException,IOException：将指定xml内容读取到当前document中.
public abstract Document newDocument()：创建一个空的文档对象
```





### Node

```java
Element、Attr、Text、ElementType 等类的父类
```

> 常用方法

```java
Node appendChild(Node newChild)：向当前节点追加一个子元素
NamedNodeMap getAttributes(): 获取当前节点的所有属性
NodeList getChildNodes(): 获取当前节点的所有子节点
Node getFirstChild(): 获取当前节点的第一个子节点
Node getLastChild(): 获取当前节点的最后一个子节点
Node getParentNode(): 获取当前节点的父节点
Node insertBefore(Node newChild,Node refChild)：将newchild插入refchild之前
Node removeChild(Node oldChild): 删除当前节点的指定子节点
Node replaceChild(Node newChild,Node oldChild): 替换子节点，并返回oldChild节点
void setTextContent(String textContent): 当前节点中的文本替换为指定文本
```







### Document

```java
当解析xml时，将xml中的数据分为：Element，Attrbute，Text 对象.
其中Element 代表 标签、Attrbute 代表标签中的属性、Text 代表标签中间的值.
而document就代表以上三个，而document也有一个父类称为node.
```

> 常用方法(包括父类的方法)

```java
Element createElement(String tagName)throws DOMException: 创建一个元素对象
Attr createAttribute(String name)throws DOMException：创建一个属性对象
Text createTextNode(String data): 创建一个文本对象
createCDATASection(String data): 创建一个自带cdata的数据
Element getDocumentElement(): 获取document子元素.
```

### 