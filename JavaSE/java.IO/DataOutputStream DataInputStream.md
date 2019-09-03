##### 数据流

对数据操作的流，一些数据比较隐私，若使用一些常用流写入则打开文件则知道隐私数据是什么，而若使用数据流可以让平常人打开看不懂，若想清楚里面是什么则使用对应的读取流读取

###### DataOutputStream

数据输出流. 写入的时候注意调用方法有的方法是按基础输出流写入，有些是底层输出流写入

区别  人看的懂	人看不懂

> 常用方法

```java
public final void writeUTF(String str)：以机器无关的方式将字符串写入基础输出流
public void write(byte[] b,int off,int len)：从位于偏移量off的指定字节数组写入len字节到底层输出流
还有一些写基本类型的方法.writeChars(String s) writeDouble(double v) writeFloat(float v) 等
```

###### DataInputStream

数据读取流.

> 常用方法

```java
public final String readUTF()：见readUTF法DataInput的一般合同。 
public final int read(byte[] b,int off,int len)：从包含的输入流读取最多len个字节的数据到字节数组
还有一些写基本类型的方法.readChars(String s) readDouble(double v) readFloat(float v) 等
```

注意：

​	1. 当调用什么writer写入的时候就建议使用什么reader读取，因为分了基础输出流写入，底层输出流写入

所以不同可能拿出来乱码

​	2. 读取的顺序要与写入的顺序一致，若不同则读取失败，因为若开始写入时使用writerFloat读取的时候使用readerUTF那肯定是不行的.



> 小案例

```java
public static void main(String[] args){
    DataOutputStream oos = new DataOutputStream("xxx.txt");
    oos.writeUTF("张三");
    oos.writeFloat(12.6L);
    oos.writeChar('男');
    //以上为写入 顺序 张三 12.6 男
    DataInputStream dis = new DataInputStream("xxx.txt");
    dis.readUTF();
    dis.readChar();
    dis.readFloat();
    //以上为读取 顺序 张三 男 12.6 顺序不一致，报错
}
```

