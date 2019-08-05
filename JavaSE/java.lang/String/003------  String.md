> String 简介

```java
一个不可变的字符序列,由多个字符组成，真正不可变的是长度.
```

> 创建方式

```java
String str1 = "abc";	String str2 = new String("abc");
```

> 区别

```java
第一种创建方式会去常量池内找是否存在该字符串.如果存在则拿过来继续用，否则添加该字符串到常量池
第二种创建方式不会去管常量池存不存在,直接在堆内开辟空间.
```

> 补充

```java
常量池1.7移入堆内，改名为元空间.
```

> 常用基本方法

```java
indexOf(int ch)"查找"、
replace(String old, String new)"替换"、
subString(int beginIndex, int endIndex)"截取"、
split(String regex)"分割"、
toUpperCase() "转为大写"  -->  toLowerCase() "转为小写"
toCharArray() "String 转 char 数组"
```





