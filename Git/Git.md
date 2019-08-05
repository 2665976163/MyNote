# Git	版本控制软件

​	分布式版本控制软件（多个用户分支）

> 命令

```java
git add:将本地文件增加到暂存区
git commit:将暂存区的内容提交到本地仓库(本地分支 master默认分支)
git push:将本地仓库的内容推送到远程仓库（远程分支）
git pull:将远程仓库（远程分支）的内容拉取到本地仓库（本地分支）
```

Git下载地址：msysgit.github.io

安装时某一选项改为：User git from gitbash only .. 

若想全局使用则配置环境(将bin内的地址复制到path中)



> 配置git：用户名与邮箱

```java
git config --global user.name "用户名"
git config --global user.email "邮箱"
```

> 查看或修改配置好的用户与邮箱

```java
C:\User\Administrator\.gitconfig 中
```





若想在本地与远程开始免密钥登陆，可以配置SSH

> 配置SSH

```java
一、先在本地配置，发送给远程
	生成命令：ssh-keygen -t rsa -C 邮箱  （默认生成在：C:\Users\Administrator\.ssh）public/private
二、配置远程
	一、点击Git头像 -> Settings -> 左边SSH and GpG... -> new SSH key
	二、Title 为 SSH名称/可随意填写
		Key为本地生成的public文件内容.(记得删除层空格)
三、测试连通性
	ssh -T git@github.com("格式不变")
    如果成功则会在.ssh文件夹内中生成Knwn_hosts文件
```





> 在本地创建项目(git)

```java
在项目的当前项目目录右键 -git bash -> -git init
本地与远程连接：
	git remote add origin 项目中的SSH或HTTP连接
```

> 第一次提交文件

```
git add * //文件-暂存区
git commit -m "描述" //暂存区 - 本地分支（默认master）
git push -u origin master	//第一次提交
```

> 第一次拷贝文件

```java
项目中 Clone or download 中点击有SSH的密钥复制
在项目文件（自创）中Git输入：git clone 密钥.(复制的)
```



> 提交

```java
git add * -> git commit -m "描述" -> git push origin master   更新：git pull
```





> 查询日志信息（历史记录）

```java
git log 可以查询到一些提交的信息，也可以说是一个一个版本
当若查询结果过长可以加一些参数：
	git log --pretty=oneline 或 git log --oneline 一行一行显示（简约的日志）
```

git log 若出现大量日志则不全显示

​	多屏显示控制：

​		空格向下翻页

​		b向上翻页

​		q退出

git reflog：（ 查询回到所有版本步数HEAD@{} ）



> 版本回退

```java
git reset --hard 版本号.(hash值) 索引值
使用^符号：只能退
	git reset --hard HEAD^
使用~符号：只能退.(解决了^不能直接退多少的问题)
    git reset --hard HEAD~15
```

除了--hard参数还有其他

​	--sort参数：仅在本地移动HEAD指针

​	--mixed参数：在本地移动HEAD指针，重置暂存区

​	--hard参数：重置暂存区、重置工作区、移动HEAD指针



> 比较文件差异

```java
git diff [文件名]
	将工作区中的文件合暂存区进行比较
git diff [本地库中历史版本][文件名]
	将工作区中的文件和本地库历史记录比较，若不带文件名则比较全部文件
```





> 分支操作

​	一、创建分支

​		git branch [分支名]

​	二、查询分支

​		git branch -v

​	三、切换分支

​		git checkout [分支名]

​	四、合并分支

​		第一步：切换到接收修改的分支(被合并，增加新内容)上：git checkout [被合并分支名]

​		第二步：执行 merge 命令  git merge [有新内容分支名]

​		解决冲突

​			一、编辑文件、删除特殊符号

​			二、把文件修改到满意的程度，保存并退出

​			三、git add [文件名]

​			四、git commit -m "日志信息"

​	五、删除分支

​		git branch -d [分支名称]





> pull	拉取

pull	=  fetch + merge

​	fetch：将远程分支下载到本地但未和本地分支合并

​	merge：合并分支







> 删除文件		rm -r 文件名

