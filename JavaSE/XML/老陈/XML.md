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



使用的类皆为org.w3c.dom 下的

### Node

```java
Element、Attr、Text、ElementType 等类的父类
```

> 常用方法

```java
Node appendChild(Node newChild) 
将节点 newChild添加到此节点的子节点列表的末尾。  
Node cloneNode(boolean deep) 
返回此节点的副本，即，作为节点的通用拷贝构造函数。 
NamedNodeMap getAttributes() 
A NamedNodeMap包含此节点的属性（如果是 Element ）或 null否则。  
NodeList getChildNodes() 
A NodeList包含此节点的所有子节点。 
Node getFirstChild() 
这个节点的第一个孩子。  
Node getLastChild() 
这个节点的最后一个孩子。   
Node getNextSibling() 
紧随该节点的节点。  
String getNodeName() 
该节点的名称取决于其类型; 见上表。  
short getNodeType() 
代表基础对象的类型的代码，如上所定义。  
String getNodeValue()
该节点的值取决于其类型; 见上表。  
Document getOwnerDocument() 
与此节点相关 Document对象。  
Node getParentNode() 
这个节点的父节点。  
Node getPreviousSibling() 
紧邻节点之前的节点。  
String getTextContent() 
此属性返回此节点及其后代的文本内容。  
boolean hasAttributes() 
返回此节点（如果它是一个元素）是否具有任何属性。  
boolean hasChildNodes() 
返回此节点是否有任何子节点。 
Node insertBefore(Node newChild, Node refChild) 
插入节点 newChild现有的子节点之前 refChild 。    
Node replaceChild(Node newChild, Node oldChild) 
替换子节点 oldChild与 newChild儿童的名单，并返回 oldChild节点。  
void setNodeValue(String nodeValue) 
该节点的值取决于其类型; 见上表。  
void setTextContent(String textContent) 
此属性返回此节点及其后代的文本内容。  
Object setUserData(String key, Object data, UserDataHandler handler) 
将对象与此节点上的键相关联。 
```







### Document

```java
当解析xml时，将xml中的数据分为：Element，Attrbute，Text 对象.
其中Element 代表 标签、Attrbute 代表标签中的属性、Text 代表标签中间的值.
而document就代表以上三个，而document也有一个父类称为node.
```

> 常用方法(包括父类的方法)

```java
Attr createAttribute(String name) 
创建给定名称的 Attr 。  
CDATASection createCDATASection(String data) 
创建值为指定字符串的 CDATASection节点。  
Comment createComment(String data) 
创建给定指定字符串的 Comment节点。  
Element createElement(String tagName) 
创建指定类型的元素。  
Text createTextNode(String data) 
创建给定指定字符串的 Text节点。  
Element getDocumentElement() 
这是一个方便属性，允许直接访问作为文档的文档元素的子节点。    
Element getElementById(String elementId) 
返回 Element具有与给定值的ID属性。  
NodeList getElementsByTagName(String tagname) 
以文件顺序返回 NodeList所有 Elements的给定标签名称，并包含在文档中。  
String getInputEncoding() 
指定在解析时用于此文档的编码的属性。  
boolean getStrictErrorChecking() 
指定是否强制执行错误检查的属性。  
String getXmlEncoding() 
一个属性指定，作为一部分 XML declaration ，本文件的编码。  
boolean getXmlStandalone() 
一个属性指定，作为一部分 XML declaration ，本文件是否是单独的。  
String getXmlVersion() 
作为XML declaration的一部分的 属性 ，指定此文档的版本号。   
Node renameNode(Node n, String namespaceURI, String qualifiedName) 
重命名类型为 ELEMENT_NODE或 ATTRIBUTE_NODE的现有节点。  
void setStrictErrorChecking(boolean strictErrorChecking) 
指定是否强制执行错误检查的属性。  
void setXmlVersion(String xmlVersion) 
作为XML declaration的一部分的 属性 ，指定此文档的版本号。  
```









### Element

> 常用方法

```java
String getAttribute(String name) 
按名称检索属性值。  
Attr getAttributeNode(String name) 
按名称检索属性节点。  
NodeList getElementsByTagName(String name) 
返回 NodeList所有子孙的 Elements具有给定标记名称，在文档顺序。  
String getTagName() 
元素的名称。  
void removeAttribute(String name) 
按名称删除属性。  
Attr removeAttributeNode(Attr oldAttr) 
删除指定的属性节点。  
void setAttribute(String name, String value) 
添加一个新属性。  
Attr setAttributeNode(Attr newAttr) 
添加一个新的属性节点。  
void setIdAttribute(String name, boolean isId) 
如果参数 isId为 true ，则此方法将指定的属性声明为用户确定的ID属性。  
void setIdAttributeNode(Attr idAttr, boolean isId) 
如果参数 isId为 true ，则此方法将指定的属性声明为用户确定的ID属性。  
```









### Attr

> 常用方法

```java
String getName() 
返回此属性的名称。  
String getValue() 
在检索时，属性的值作为字符串返回。  
boolean isId() 
返回此属性是否已知为类型ID（即  
void setValue(String value) 
在检索时，属性的值作为字符串返回。  
```

