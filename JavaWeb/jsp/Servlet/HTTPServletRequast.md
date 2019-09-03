> 方法

```java
Locale languageType=request.getLocale();//获取用户语言
String localIp=request.getLocalAddr();//获取本地ip
int localPort=request.getLocalPort();//获取本地的端口
String localName=request.getLocalName();//获取本地计算机的名字
String remoteIp=request.getRemoteAddr();//获取客户端的ip
int remotePort=request.getRemotePort();//获取客户端的端口号
String serverName=request.getRemoteHost();//获取远程计算机的名字
System.out.println("语言类型->"+languageType);
System.out.println(localName+" "+serverName);
System.out.println(localIp+":"+localPort+" "+remoteIp+":"+remotePort);

public void doGet(HttpServletRequest request, HttpServletResponse response){
    String requestURL=request.getRequestURL().toString();//获取除参数之外的地址信息
    String requestURI=request.getRequestURI();
    String queryString=request.getQueryString();
    System.out.println("请求的地址->"+requestURL);
    System.out.println("请求的地址后的信息->"+queryString);
}
```

