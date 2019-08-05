#### NIO（new IO）

> NIO  与  IO  的区别

```java
1.我们之前使用的IO是面向流，而NIO是面向缓冲区.
2.IO 读取 属于 阻塞式，而NIO可以非阻塞式.
3.NIO支持选择器，而IO不支持.
```

> NIO分类

```java
NIO分为俩类：直接缓冲区  与  非直接缓冲区.
```

> 缓冲区

```java
缓冲区用于存储数据，基本类型也有自己的缓冲区(除boolean外)
比如：ByteBuffer,CharBuffer,IntBuffer,DoubleBuffer,ShrotBuffer,LongBuffer,FloatBuffer..等
```

> 缓冲区的字段

```java
要想了解缓冲区必须要了解缓冲区中的四个字段:
	mark <= limit <= position <= capacity;
mark(标记)  		//标记当前缓冲区状态
position(数据个数)  //缓冲区中的数据数量
limit(可读可写最大值)   //当向缓冲区写入数据时最大可写入值,当向缓冲区读取数据时最大可读取值
capacity(容量)	//缓冲区的容量
```

> 缓冲区的常用方法(ByteBuffer 举例)

```java
//方法1.
	allocate(int capacity)  //用于给缓冲区分配空间大小
//方法2.
	array() //将该缓冲区中的数据以byte数组返回
//方法3.
     flip() //可以理解为"切换为读模式" (position = 0  limit = 缓冲区中数据数量)
//方法4.
     clear() //将缓冲区中的数据"清空"，并不是真的清空,而是position 为 0 limit 为 数组容量
        	//可以重头重新写数据,原数据没有被清空但是已经不清楚有多少数据了,可以理解为"切换写模式"
//方法5.
     get() //获取position目标下的值 然后position递增(搭配flip使用)
//方法6.
     isDirect() //判断当前缓冲区是否为直接缓冲区
//方法7.
     mark() //标记当前缓冲区position状态
//方法8.
     reset()  //调用此方法将回到mark标记时的状态
//方法9.
     rewind()  //位置(position)设置为零，标记被丢弃,建议搭配flip使用
```

> 非直接缓冲区（概续）

```java
非直接缓冲区就是说不能直接操作缓冲区，而是得通过通道传递缓冲区中的数据.
```

> 通道

```java
IO中的流可以直接传递数据，而NIO通过通道传递数据，通道就相当于轨道，而缓冲区就等于火车.
```

```java
Channel 
   |---FileChannel
   |---SocketChannel 
   |---ServerSocketChannel 
   |---DatagramChannel
//第一个用于本地建立通道，其他均为网络 .
```

> FileChannel 中 建立传输(FileInput 与FileOutputStream也提供了FileChannel的创建，但该例没有使用)

```java
//1.创建缓冲区
ByteBuffer btBuffer = ByteBuffer.allocate(1024);

//读区===============================================================================
//2.创建通道(有很多种方式创建)
/*
	暂时通过open创建 open(Path path，OpenOption...)
	open的第一个参数：
		地址：
        Path 有一个工具类 名为 Path 该类有俩个方法 get(URL)  get(String first, String... more)
        稍后我们使用第二个方法 first 与 more 好像最后是相加起来的 如get("F:\\","io10g","\\a.txt")
	open的第二个参数：
		模式：
		OpenOption 是接口 有一个用得到的已知子类StandardOpenOption(该类为枚举类型)
		常用字段：READ CREATE WRITE CREATE_NEW APPEND 
		read 支持读模式，write支持写模式，CREATE 当文件不存在创建,存在覆盖
		CREATE_NEW 存在报错，不存在创建	
		APPEND：文件打开 WRITE访问，则字节将被写入文件的末尾而不是开头
*/
//该通道支持读模式
FileChannel fChannel = FileChannel.open(Paths.url("地址"),StandardOpenOption.READ);
//3.将通道中地址的数据读写到缓冲区中
fChannel.read(btBuffer);
//打印
System.out.println(new String(btBuffer,0,fChannel.size()));

//写区==================================================================================
//该通道支持写模式,创建文件..
FileChannel fileChannel1 = FileChannel.open(Paths.get("目标地址"),
                                            StandardOpenOption.WRITE,StandardOpenOption.CREATE);
//将缓冲区切换为读模式,好供我们通道将该缓冲区数据写入通道
byteBuffer.flip();
//写入
fileChannel1.write(byteBuffer);
//既然写入了，那么若资源已经不需要再使用则清空缓冲区.
byteBuffer.clear();
```

> 直接缓冲区(速度非常快)

```java
直接缓冲区就是缓冲区直接映射本地文件然后缓冲区之间直接操作.
```

```java
//创建读通道
FileChannel input = FileChannel.open(Paths.get("F:\\io10g\\a.txt"), StandardOpenOption.READ);
//通过读通道获取MappedByteBuffer缓冲区对象 
MappedByteBuffer inputBuffer = input.map(FileChannel.MapMode.READ_ONLY,0,input.size());
//创建byte数组用于接收数据文件
byte[] bytes = new byte[(int)input.size()];
//get将地址中的文件数据读取到bytes数组中
inputBuffer.get(bytes,0,(int)input.size());
//创建写通道
FileChannel Output = FileChannel.open(Paths.get("F:\\io10g\\b.txt"),StandardOpenOption.READ,StandardOpenOption.WRITE,StandardOpenOption.CREATE);
//通过写管道获取MappedByteBuffer缓冲区对象 
MappedByteBuffer outputBuffer = Output.map(FileChannel.MapMode.READ_WRITE,0,input.size());
//put将写入bytes中的数据读取出来
outputBuffer.put(bytes);
```

> 分散读取  与  聚集写入

```java
//缓冲区非常强大 Buffer
//数据容器1
ByteBuffer btBuffer1 = ByteBuffer.allocate(1024);
//数据容器2
ByteBuffer btBuffer2 = ByteBuffer.allocate(1024);
//数据容器3
ByteBuffer btBuffer3 = ByteBuffer.allocate(1024);
//数据容器4
ByteBuffer btBuffer4 = ByteBuffer.allocate(1024);
//可以开启多个线程对以上缓冲区同时读取，再合并
ByteBuffer[] btBuffer1 = new ByteBuffer[]{btBuffer1,btBuffer2,btBuffer3,btBuffer4};
```

> 单线程举例

```java
FileChannel input = FileChannel.open(Paths.get("F:\\io10g\\a.txt"), StandardOpenOption.READ);
ByteBuffer btBuffer1 = ByteBuffer.allocate(1024);
ByteBuffer btBuffer2 = ByteBuffer.allocate(1024);
ByteBuffer btBuffer3 = ByteBuffer.allocate(1024);
ByteBuffer btBuffer4 = ByteBuffer.allocate(1024);
input.read(btBuffer1);
input.read(btBuffer2);
input.read(btBuffer3);
input.read(btBuffer4);
ByteBuffer[] buffers = new ByteBuffer[]{btBuffer1,btBuffer2,btBuffer3,btBuffer4};
FileChannel Output = FileChannel.open(Paths.get("F:\\io10g\\b.txt"),StandardOpenOption.WRITE,StandardOpenOption.CREATE);
for (int i = 0; i <buffers.length ; i++) {
    buffers[i].flip();
}
Output.write(buffers);
```

> 字符集

```java
//查询本机支持的字符集
SortedMap<String,Charset> charset = Charset.availableCharsets();
charset.forEach((a,b)->{
    System.out.println("   键"+a+"   值"+b);
});
```

```java
//创建字符集对象
Charset charset = Charset.forName("编码");
```

> 编码器	CharsetEncoder encoder = charset.newEncoder();

```java
Charset charset = Charset.forName("GBK");
CharBuffer charBuffer = CharBuffer.allocate(1024);
charBuffer.append("今晚打老虎");
CharsetEncoder encoder = charset.newEncoder();
charBuffer.flip();
System.out.println(charBuffer.length());
```

> 解码器	CharsetDecoder decoder = charset.newDecoder();

```java
ByteBuffer decoder = encoder.encode(charBuffer);
System.out.println(charset.newDecoder().decode(b));
```

> 阻塞式

```java
当客户端读取本地数据给服务端发送请求，然后服务端接收请求但不清楚客户端到底发送完没，一直等待.
```

> 举例

```java
new Thread(()->{
    try {
         FileChannel output = FileChannel.open(Paths.get("F:\\io10g\\b.txt"),                                    		StandardOpenOption.WRITE,StandardOpenOption.CREATE);
         ServerSocketChannel sChannel = ServerSocketChannel.open();
         sChannel.bind(new InetSocketAddress(8999));
         SocketChannel socketChannel = sChannel.accept();
         int len;
         ByteBuffer btBuffer = ByteBuffer.allocate(1024);
         while ((len = socketChannel.read(btBuffer)) != -1){
              btBuffer.flip();
              output.write(btBuffer);
              btBuffer.clear();
         }
   } catch (IOException e) {
         e.printStackTrace();
   }

}).start();

Thread.sleep(200);
FileChannel output = FileChannel.open(Paths.get("F:\\io10g\\a.txt"),StandardOpenOption.READ);
SocketChannel sChannel = SocketChannel.open(new InetSocketAddress("172.20.10.4",8999));
int len;
ByteBuffer btBuffer = ByteBuffer.allocate(1024);
while ((len = output.read(btBuffer)) != -1){
     btBuffer.flip();
     sChannel.write(btBuffer);
     btBuffer.clear();
}
//sChannel.shutdownOutput(); 
//若此处不加shutdownOutput则服务器一直不清楚客户端数据传输完没，服务端一直等待
```



> 非阻塞式

```java

```

