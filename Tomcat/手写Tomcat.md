# 手写tomcat

## 第一节

1. 了解怎么接收客户端的请求
2. 了解怎么响应客户端

使用网络编程，接收客户端发送的请求

```java
//创建服务器Socket对象
ServerSocket socket = new ServerSocket(80);
//等待连接
socket.accept();
byte[] rquestValue = new byte[1024*1024];
//将客户端发送的请求写入字节数组
int len = socket.getInputStream(rquestValue);
//打印客户端发送的请求信息
System.out.println(new String(rquestValue,0,len));
```

使用网络编程，接收完信息，反馈给客户端

```java
StringBuilder htmlContent = new StringBuilder();
htmlContent.append("<html>");
htmlContent.append("<head>");
htmlContent.append("<title>").append("来自服务器..").append("</title>");
htmlContent.append("</head>");
String content = "<h1>请求成功！</h1>-----来自服务器的响应";
htmlContent.append("<body>").append(content).append("</body>");
htmlContent.append("</html>");
OutputStream outputStream = socket.getOutputStream();
outputStream.write(htmlContent.toString().getBytes("GBK"));
```

> 典型的响应协议

```txt
典型的响应协议
1、状态行:HTTP/1.1 200 0K
2、请求头:Date:Mon,31Dec209904:25:57GMT
Server:shsxt Server/0.0.1;charset-GBK
Content-type:text/html
Content-length:39725426
3、请求正文(注意与请求头之间有个空行)
xxxxxx
```

```java
StringBuilder htmlContent = new StringBuilder();
htmlContent.append("<html>");
htmlContent.append("<head>");
htmlContent.append("<title>").append("来自服务器..").append("</title>");
htmlContent.append("</head>");
String content = "<h1>请求成功！</h1>-----来自服务器的响应";
htmlContent.append("<body>").append(content).append("</body>");
htmlContent.append("</html>");
StringBuilder httpContent = new StringBuilder();
String blank = " ";
String CRLF = "\r\n";
httpContent.append("HTTP/1.1");
httpContent.append(blank).append("200");
httpContent.append(blank).append("OK");
httpContent.append(CRLF);
httpContent.append("Date:").append(new Date()).append(CRLF);
httpContent.append("Server:").append("znsd Server/0.0.1;charset=GBK").append(CRLF);
httpContent.append("Content-type:text/html"+CRLF);
httpContent.append("Contentlength:");
httpContent.append(htmlContent.toString().getBytes().length);
httpContent.append(CRLF).append(CRLF);
httpContent.append(htmlContent.toString());
OutputStream outputStream = socket.getOutputStream();
outputStream.write(httpContent.toString().getBytes("GBK"));
```

