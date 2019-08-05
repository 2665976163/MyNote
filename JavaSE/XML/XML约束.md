# XML约束

> 概念

​	若没有约束XML格式就是一盘散沙，想怎么写就怎么写，若定义约束则只能按照约束要求的写，XML约束分为俩种：DTD、Schema

### DTD 约束

> 概念

```java
语法自成一派，早期出现的，可读性比较差.
```

> 外部DTD导入

```java
//本地导入
<!DOCTYPE 根元素 SYSTEM "地址/文件名">
```

> 内部DTD

```java
<!DOCTYPE 根元素 [
    <!ELEMENT stus	(stu)>
    <!ELEMENT stu	(name,age)>
    <!ELEMENT name	(#PCDATA)>
    <!ELEMENT age	(#PCDATA)>
]>
//根元素指的是xml中的头元素
```

（不写空格会有错误提示，但不影响）具体限制内容元素约束参考

https://www.w3school.com.cn/dtd/dtd_elements.asp

标签旁加* 0或多个 ?0或1个 +一个或多个

#### 属性详情

https://www.w3school.com.cn/dtd/dtd_attributes.asp

```txt
<!ATTLIST 绑定的标签名 属性名 属性类型 默认值>
```



### Schema 约束

> 概念

```java
其实就是一个XML，XML解析器解析起来比较方便，是为了取代DTD，但Schema约束内容又比DTD要多，所以目前为止没有正真意义上的取代DTD。
```

创建Schema约束 .xsd

> 模板

```java
<?xml version="1.0" encoding="UTF-8"?>
<schema xmlns="http://www.w3.org/2001/XMLSchema"
           targetNamespace="http://www.w3school.com.cn//node.xsd"
           elementFormDefault="qualified">
</schema>
```

> 解析

```java
xmlns:名称空间/命名空间	.xsd约束.xml w3c约束当前.xsd文件,xmlns为w3c的约束	(写死)
targetNamespace：目标名称空间	与当前内约束标签绑定 .(当其他xml需要约束时会用到目标名称空间)
elementFormDEfault：默认模式
```

> 举例（Schema约束）

```java
<?xml version="1.0" encoding="UTF-8"?>
<schema xmlns="http://www.w3.org/2001/XMLSchema"
        targetNamespace="http://www.w3school.com.cn//students.xsd"
    elementFormDefault="qualified">
    <element name="students">
        <complexType>
            <sequence>
                <element name="student">
                    <complexType>
                        <sequence maxOccurs="3" minOccurs="2">
                            <element name="name" type="string"></element>
                            <element name="age" type="int"></element>
                        </sequence>
                    </complexType>
                </element>
            </sequence>
        </complexType>
    </element>
</schema>
```

> XML引用多个Schema约束

```java
<?xml version="1.0" encoding="UTF-8"?>
<bb:students
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:aa="http://www.w3school.com.cn//node.xsd"
        xmlns:bb="http://www.w3school.com.cn//students.xsd"
        xsi:schemaLocation="http://www.w3school.com.cn//src/node.xsd  node.xsd
        http://www.w3school.com.cn//students.xsd  students.xsd"
>
    <bb:student>
        <bb:name></bb:name>
        <bb:age>123</bb:age>
        <bb:name></bb:name>
        <bb:age>123</bb:age>
    </bb:student>
</bb:students>
```

> 解析

```java
xmlns:xsi：XMLSchema实例（写死）
xmlns:aa：当引入多个schema时需要一个别名，aa，bb就是对应schema约束的别名
xsi:schemaLocation:目标名称空间 约束地址	以上是idea的src目录索引不需要绝对路径
```

详情：https://www.w3school.com.cn/schema/schema_howto.asp