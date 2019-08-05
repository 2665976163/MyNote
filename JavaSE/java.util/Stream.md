### Stream 流

> 概念

```txt
Java 8 API添加了一个新的抽象称为流Stream，可以让你以一种声明的方式处理数据。
Stream 使用一种类似用 SQL 语句从数据库查询数据的直观方式来提供一种对 Java 集合运算和表达的高阶抽象。
Stream API可以极大提高Java程序员的生产力，让程序员写出高效率、干净、简洁的代码
这种风格将要处理的元素集合看作一种流， 流在管道中传输， 并且可以在管道的节点上进行处理， 比如筛选， 排序，聚合等。
元素流在管道中经过中间操作(intermediate operation) 的处理，最后由最终操作(terminal operation)得到前面处理的结果。
```

### 什么是 Stream？

Stream（流）是一个来自数据源的元素队列并支持聚合操作

元素是特定类型的对象，形成一个队列。 Java中的Stream并不会存储元素，而是按需计算。

> 数据源 

```java
流的来源：可以是集合，数组，I/O channel， 产生器generator 等。
```

> 聚合操作 类似SQL语句一样的操作， 比如filter, map, reduce, find, match, sorted等。

和以前的Collection操作不同， Stream操作还有两个基础的特征：

> Pipelining:

```txt
中间操作都会返回流对象本身。 这样多个操作可以串联成一个管道， 如同流式风格（fluent style）。 这样做可以对操作进行优化， 比如延迟执行(laziness)和短路( short-circuiting)。
```

> 内部迭代

```txt
以前对集合遍历都是通过Iterator或者For-Each的方式, 显式的在集合外部进行迭代， 这叫做外部迭代。 Stream提供了内部迭代的方式， 通过访问者模式(Visitor)实现。
```

### 生成流

```txt
stream() − 为集合创建串行流。(单线程)
parallelStream() − 为集合创建并行流。（多线程）
```

常用方法

> allMatch(Predicate<? super T> predicate)  与  anyMatch(Predicate<? super T> predicate)
>

```java
Predicate 函数式接口 重写 test(T t) 方法 test方法中存放逻辑判断
ArrayList<String> list {"小二,张三,李四,王五,赵六,周七,魁八"}; 
					//👆 只是集合内有这些元素,集合语法是错误的.
如 : 
Stream stream = list.parallelStream();
//allMatch 需要的参数为 Predicate 若不想写类可以使用 lambada表达式
stream.allMatch((t) -> {
	if(!t.contains("小")){	//<-- 公式
        System.out.println(t);
	}
	return true;
});
/*
	结果：张三,李四,王五,赵六,周七,魁八
*/
/*
过程：
    allMatch方法会将集合副本元素通过循环一个一个传入公式中
    如果输入参数(t)匹配谓词(公式)true，当集合为空时为 false 
*/
//也可以写类
stream.allMatch(new Predicate(){
   public boolean test (t) -> {
	if(!t.contains("小")){	//<-- 公式
        System.out.println(t);
	}
	return true;
   }
});
另外一个方法就不说了,都差不多.
/*
方法描述：
    allMatch 该方法会将集合中所有数据只要匹配公式则打印.
    anyMatch 打印集合中任意匹配的一个.
*/
```

> builder()

```java
构建一个Stream内置接口Builder的实例.
该Builder中可以添加元素然后通过该对象的build()方法返回Stream对象
Stream就可以对Builder添加好的元素做操作.
Builder ss = Stream.builder(); 
ss.add(123);
ss.add(123);
ss.add(123);
ss.add(123);
ss.add(123);
Stream ww = ss.build();
ww.forEach((t)->{
   System.out.println(t);
});
```

> concat(Stream <? extends T>a,Stream <? extends T>b)

```java
将俩个Stream流合并成一个
ArrayList<Integer> list = new ArrayList();
for(int i=0;i<1235;i++){
    list.add(i);
}
Builder ss = Stream.builder(); 
ss.add("iloveyou");
ss.add("iloveyou");
ss.add("iloveyou");
ss.add("iloveyou");
ss.add("iloveyou");
Stream ww = ss.build();
Stream dd = list.parallelStream();
Stream cc = Stream.concat(ww, dd);
cc.forEach((a)->{
   System.out.println(a);
});
```

> distinct() 去重

```java
Builder ss = Stream.builder(); 
ss.add("iloveyou");
ss.add("iloveyou");
ss.add("iloveyou");
ss.add("iloveyou");
ss.add("iloveyou");
Stream ww = ss.build();
ww.distinct().forEach((a)->{
   System.out.println(a);
});
```

> filter(Predicate<? super T> predicate)

```java
与上面的allMatch区别很大,该方法返回一个Stream，而allMatch返回boolean .
该方法可以实现单线程并发(很重要)并且线程安全(若是并行流则不保证线程安全).
Builder ss = Stream.builder(); 
ss.add("iloveyou");
ss.add("iloveyou");
ss.add("iloveyou");
ss.add("iloveyou");
ss.add("iloveyou");
Stream ww = ss.build();
ww.filter((t) ->{
   if(((String) t).contains("ilo")) {
       System.out.println("==="+t);
   }
   return true;
}).filter((t) ->{
    if(((String) t).contains("ilo")) {
        System.out.println("www"+t);
    }
    return true;
}).filter((t) ->{
    if(((String) t).contains("ilo")) {
        System.out.println("sss"+t);
    }
    return true;
}).forEach(a->{
    System.out.println(a);
});
```

列举以上这几个,详情查api

```java
Comparator  称为消费  重写 test(T t);	//输出出来
Predicate 条件判断 可以筛选你想要的 重写 test(T t); //bir：我想查询集合中姓张的
当中有一个写了过程，过程很重要！
```

> 必记单词

```java
concat  //拼接
count	//个数
distinct	//去重
limit	//限制个数
max	//最大值
min	//最小值
map	//映射
reduce	//聚合
```

