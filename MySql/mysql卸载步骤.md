# mysql完全卸载教程（图文详细）

若图片不显示请连接网络

快捷键win+r输入regedit进入注册表，找到HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Eventlog\Application\MySQL文件夹删除 
![这里写图片描述](https://img-blog.csdn.net/2018080722103725?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMTQwNzQx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 
删除HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\Services\Eventlog\Application\MySQL文件夹。 
删除HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application\MySQL的文件夹。 
注册表里没有这两个文件，就不用删除了 
然后打开任务管理器，关掉MySQL： 
![这里写图片描述](https://img-blog.csdn.net/20180807221800782?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMTQwNzQx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 
然后是删除MySQL的安装位置： 
我的是默认路径C盘：C:\Program Files\MySQL\MySQL Server 5.7 
![这里写图片描述](https://img-blog.csdn.net/20180807221826909?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMTQwNzQx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后是另一个地方的：C:\Program Files (x86)\MySQL 
![这里写图片描述](https://img-blog.csdn.net/20180807221946630?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMTQwNzQx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 
还有一个隐藏文件下的，先点击【查看】->勾选【隐藏的项目】 
![这里写图片描述](https://img-blog.csdn.net/2018080722210130?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMTQwNzQx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
可以看到一个名为ProgramData的文件夹，点开找到里面的MySQL，删除就行了 
![这里写图片描述](https://img-blog.csdn.net/20180807222253308?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMTQwNzQx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)