IO流

## 1.文件的创建、写入、刷新、关闭

> FileWriter	（创建	参考②）

①	创建一个FileWriter，该对象一被初始化就要明确被操作的文件

②	如果传入的文件不存在将会创建一个文件，如果存在那么将会覆盖原来的文件

③	其实FileWriter就是在明确数据要存放的目的地（文件）

```java
FileWriter Temp = new FileWriter("文件名");
```



> write()方法	（写入  参考①）

①	调用write方法，将字符串写入文件(流)中.

```java
Temp.write("你好");
//文本内会写入"你好"；
```



> flush()方法	（刷新   参考①、②）

①	刷新流对象中缓冲中的数据

②	将数据刷新到目的地（文件）中

```java
Temp.flush();
```



> close()方法	（刷新且关闭   参考①）

①	关闭流资源(不能再写入)，但是关闭之前会刷新一次内部的缓冲中的数据（隐藏调用一次flush方法）;

```java
Temp.close();
```



> 注意：flush跟close的区别 flush刷新之后还能继续使用   close刷新之后会将流关闭
>
> 严重注意：
>
> ①	创建FileWriter及以上方法都需要抛出异常main方法后加上throws  IOException   ||   将其放入try















## 2.IO流异常处理方式

```java
public static void main(String args[]){
    FileWrite temp = null;
    try{
        temp = new FileWrite("a.text");
        temp.flush();
    }catch(IOException e){
        System.out.println("cacth："+e);
    }//如果流执行完了(开启了,不是空的)我还要关闭流
    finally{
        //if判断的是如果我的对象是空的那么我就不用关闭流
        //因为如果是空的那么这个对象就是不存在的也不存在关闭流的方法
        if(temp!=null){
            try{
                temp.close();
            }catch(IOException e){
                System.out.println(e);
            }
        }
    }
}
```







































## 3.对文件的续写以及换行

> FileWriter

![TileWriter续写](Img\TileWriter续写.png)

```java
FileWriter Temp = new FileWriter("a.txt",true);
//以上构造函数内参数二如果是true就会将数据写入文件末尾处，而不是写入文件开始处
```





> \r\n  换行符

```java
public static void main(String args[]) throws IOException{
    FileWriter Temp = new FileWriter("a.txt",true);
    Temp.write("\r\n你好");
    Temp.close();
}
//             ↓  运行结果
```

![TileWriter续写bak](Img\TileWriter续写bak.png)















































## 4.文本文件读取

> FileReader(继承Reader类)

```java
FileReader temp = new FileReader("要读取的文件名");
```





> read () 	读取方法 ①

```java
int a = temp.read();	//只能读取一个字符

while((a = temp.read())!=-1){	//可以利用while读取文件全部内容 当到流末尾返回为-1
    System.out.print(a);
}
```

![TileWriter续写bak](Img\FileReader读取read.png)





> read (char[] cbuf)	  读取方法②

```java
char[]arr = new char[1024];	//标准存储数(2kb);
int Count = 0;//(读取的内容数量)
while((Count = temp.read(arr))!=-1){//如果读取的数量返回不是-1那么就打印  当到流末尾返回为-1
    System.out.print(new String(arr,0,Count)); //利用String方法从0-Count
}
```

![TileWriter续写bak](Img\FileReader读取read2.png)

































## 5.字符流的缓冲区

一、缓冲区的出现提高了对数据读写的效率

二、对应类

​		① 	BufferedWriter

​		②	BufferedReader

三、缓冲区要结合流才可以使用(先创建FileWriter)

四、在流的基础上对流的功能进行了增强(缓冲区的新方法)



> ##### BufferedWriter的介绍

```text
![BufferedWirter](Img\BufferedWirter.png) //API: BufferedWriter 介绍
![BufferedWirter](Img\BufferedWirter函数.png) //API: BufferedWriter 新方法、继承父类方法
```



> BufferedWriter

```java
/*
	缓冲区的出现是为了提高流的效率
	所以创建缓冲区之前，需要先有流对象，要不然没有缓冲对象
*/
//创建一个 "字符写入流对象".
FileWriter Copy = new FileWriter("a.txt");  
//为了提高"字符写入流"的效率,加入了缓冲技术
//只要将需要被提高效率的流对象作为参数传递给缓冲区的构造函数即可.
BufferedWriter temp = new BufferedWriter(Copy); 
//写入流与缓冲区起了关联，那么我就可以使用缓冲区的方法
//并且BufferedWriter继承了Writer我也可以使用Writer的方法
temp.close();
//关闭缓冲区就相当于关闭流,所以就不需要再Copy.close();


/*
	缓冲区的新方法就是换行符,兼容Windows,Linux的换行符	
	Windows的换行是\r\n，而在Linux\r就是多余的转义符
	使用方法： temp.newLine(); //写入一个兼容行分隔符
*/
```





> BufferedReader的介绍

```java
![BufferedReader](Img\BufferedReader.png) //API: BufferedReader 介绍
![BufferedReader](Img\BufferedReader函数.png) //API: BufferedReader 新方法、继承父类方法
```

```java
FileReader fr = new FileReader("a.txt");
BufferedReader bufr = new BufferedReader(fr);
String line = null;
while((line=bufr.readLine())!=null){
   temp.readLine(line);    //缓冲区读取一行的新方法,FileWriter没有这个方法,当流到末尾返回为null
   temp.newLine();   //readLine不会读取换行符,所以要加上newLine(兼容换行符)
   temp.flush();
}
temp.close();
bufr.close();
```

> readLine（读取一行）的原理

```java
//无论读取一行,获取读取多个字符,其实最终还是在硬盘上一个一个读取
//所以最终使用的还是read方法一次读取一个字符的方法

//readLine 把数据临时存起来,当读到换行符的时候就直接把读取的返回出去,也是一个一个读取
//(传递数组的方式也是这样一个一个读)
//自定义读取一行的代码 ↓
//readLine的底层是数组,因为麻烦所以我们使用StringBuffer
class MyBufferedReader
{
	private FileReader r;
	public MyBufferedReader(FileReader r){
		this.r = r;
	}
	public String MynewLine()throws IOException{
		StringBuffer sb = new StringBuffer();
		int ch = 0;
		while((ch=r.read())!=-1){
			if(ch=='\r')
				continue;
			if(ch=='\n')
				return sb.toString();
			else
				sb.append((char)ch);
		}
		if(sb.length()!=0)
			return sb.toString();
		return null;
	}
	public void MyClose()throws IOException{
		r.close();
	}
}
```











































## 6.获得当前行号与设置当前行号

> BufferedReader的直接已知子类LineNumberReader

```txt
该类拥有BufferedReader的所有方法
比如：
	read() 读取单个字符。 
 	read(char[] cbuf, int off, int len) 将字符读入数组中的某一部分。 
 	readLine()  读取文本行。 
	作为子类当然拥有自己独特的方法
	就是获得当前行号与设置当前行号让打印数据有行数显示
	getLineNumber()   获得当前行号。
	setLineNumber(int lineNumber) 设置当前行号。//参数为你要设置为当前的行号为多少开始
	
	//举例：  ↓
	FileReader fr = new FileReader("a.txt");
	LineNumberReader lnr = new LineNumberReader(fr);
	String line = null;
	lnr.setLineNumber(100);//设置当前行号从100开始
	while((line=lnr.readLine())!=null){
        System.out.println(lnr.getLineNumber()+"："+line);//获取当前行号从1开始
	}
```













































## 7.字节写入不需要刷新与读取准确文件内容个数

这俩个都是通过操作字节写入或读取

> FileOutputStream

```txt
通常写入文件内容需要刷新（flush()）但是这个的独特方法是直接使用最底层不需要缓冲区直接将字节写入文件
它的所有write()内的参数都得是byte类型
FileOutputStream fos = new FileOutputStream("a.txt")；
			    fos.write("abc".getBytes());//getBytes是将字符串转换为byte数组
			    fos.close();
```





> FileInputStream

```txt
含有一个特殊获取文本内容个数；比如文本内容有abcde那么它会返回 5；
FileInputStream fis = new FileInputStream("a.txt");
		byte[] arr = new byte[fis.available()];
		fis.read(arr);
		System.out.println(new String(arr));
		
		//注意事项：如果文件过大还是建议使用1024,不建议使用available方法
```



































## 8.利用字节流拷贝图片

> FileInputStream（读取）、FileOutputStream（写入）

```txt
FileInputStream fis = new FileInputStream("1.png");//选择要复制的文件
FileOutputStream fos = new FileOutputStream("2.png")//粘贴的文件名
byte[]buf = new byte[1024];
int len = 0;
while((len = fis.read(buf))!=-1){
    fos.write(buf,0,len);
}
fis.close();
fos.close();

1.提问字节流可以拷贝图片字符流可不可以？
		字符流可以拷贝图片但是会造成图片无法正常看见.
		字符流只用于读取或写入文本文件,
		字节流可以用来拷贝媒体文件.
```



利用以上类的缓冲区拷贝音乐文件

> BufferedInputStream、BufferedOutputStream

```txt
BufferedOutputStream fos = new BufferedOutputStream(new FileOutputStream("3.mp3"));
BufferedInputStream fis = new BufferedInputStream(new FileInputStream("1.mp3"));
	byte[] buf = new byte[1024];
	int len = 0;
	while((len=fis.read(buf))!=-1){
		fos.write(buf,0,len);
	}
	fis.close();
	fos.close();
```

> 字节流专门拷贝媒体文件，字符流拷贝文本文件





































## ====以上总结====

> 字符流

```txt
基本读取：FileReader
					拥有父类常用方法：
							read();	//基本读取数据字符
							close();	//关闭流
基本写入：FileWriter
					拥有父类的常用方法：
         					  write();	//基本写入
							 flush();  //刷新该流的缓冲
							 close();  //刷新并关闭该流
							 getEncoding();  //返回此流使用的字符编码的名称(utf-8)

带新方法读取：BufferedReader
					拥有FileReader的以上方法
					带有一个读取一行的方法：readLine();
			
带新方法写入：BufferedWriter
					拥有FileWriter的以上方法
					带有一个兼容换行符方法：newLine();//Windows与Linux换行符不同这个方法兼容换行
					
以上读取的类型是char类型.
```

> 字节流

```txt
基本读取：FileInputStream
					拥有常用方法：
							read();	//基本读取数据字节
							close(); // 关闭此文件输入流并释放与此流有关的所有系统资源
							available() //读取文本内容个数(新方法)
			
基本写入：FileOutputStream
					拥有常用方法：
							write();// 不需要刷新直接向文件写入内容
							close();// 关闭此文件输入流并释放与此流有关的所有系统资源
							

```

