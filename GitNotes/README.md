# GIT学习笔记

在分享我的学习笔记之前，推荐各位爱学习的宝宝们关注一下`廖雪峰`老师的网站，记录着更为详细的知识点，在这里我只是结合自身所学罗列出一些比较常用的方法，供各位爱学习的宝宝使用，如果您喜欢的话，希望您动动您可爱的小手指Star一下哦，谢谢！

学习更多内容前往 [GIT 教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000) 

## Git总结

  `简介`
  
> * 说明：下列文本性内容部分来自廖雪峰的网站，一部分来自公司实战，一部分来自官方网站
> * 发布日记，杂文，所见所想
> * 撰写发布技术文稿（代码支持）

## 学习网站

> 1. http://www.liaoxuefeng.com/   Git的完整学习教程
> 2. https://git-for-windows.github.io/   windows上安装msysgit，内部包含模拟环境和Git
> 3. 如果英文不好，可以使用中文版，然后直接使用图形化界面Git Gui，而不使用Git bash
> 4. `其他学习网址：`
  * https://blog.cnbluebox.com/blog/2014/04/15/gitlabde-shi-yong/
  * http://www.oschina.net/translate/10-tips-git-next-level
  
### 1. 基本命令

   **初始化设置**
   
   *配置本机的用户名和Email地址*

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

**创建版本库(仓库)**

```
版本库又叫仓库(repository)，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除都能被跟踪。
在合适的位置直接鼠标右键创建一个空目录作为仓库，然后从Git-Bash命令行进入到该目录，或者也可以使用命令行创建空目录，再进入到该空目录中。  
以下给出创建并初始化git仓库的代码：  
进入到仓库的位置，我将仓库放在了C:\GitStudy\git-repositories目录下，注意，使用cd命令进入到目录中时，在Git-Bash中应该使用斜线”/”，  
而不是反斜线”\”  
```

```
$ cd C:/GitStudy/git-repositories
$ mkdir new_repository_1           创建新的目录
$ cd new_repository_1              进入到创建的目录
```

**使用init命令将当前目录初始化为Git仓库**

```
$ git init
Initialized empty Git repository in C:/GitStudy/git-repositories/new_repository_1/.git/
(显示信息意思为：初始化了一个空的Git仓库，new_repository_1目录下多了一个.git目录，时用来管理版本库的)
将数据提交到git仓库（本地仓库）
```

> 第一步：**添加文件**

```
$ git add .        添加所有的文件、文件夹
$ git add <file>   添加指定名称的文件,<>内部写文件全称
注：如果文件没有做出任何修改，则默认不会添加任何文件
```

> 第二步：**提交文件**

```
$ git commit –m “commit info”    提交本次事务，即将add的文件提交到git仓库，引号内部表示本次提交的提示信息
```

**查询提交状态**

```
$ git status       显示提交的状态：已经添加，等待提交事务的文件(绿色字体表示)；已经改变但是没有添加(not staged)的文件(红色字体表示)；
查询该文件和git仓库中的文件的区别，即做了什么修改
$ git diff <文件全称>      如果已经add了，就打印不出有什么修改了，这一步骤应该在add之前，即添加之前可以用来看看做了什么修改。
```

**打印历史记录**

```
$ git log
Commit xxx              commit id 版本号
Author:xxx<xxx@xxx.com> 提交人和邮箱
Date：xxx                提交的时间
    XXXXXXXXXXXXXX      提交的信息(所以说，提交信息很重要！！！)
$ cat <文件全名称>      显示整个文件的内容
```

**版本回退**

```
$ git reset --hard head^
在Git中，HEAD表示当前版本，就是最新提交的版本，即使用git log打印出来的位于第一位的版本，上一个版本就是HEAD^，上上个版本就是HEAD^^，  
当前向上100个可以写成HEAD~100。当然，还有一种方式就是直接使用commit id来代替HEAD^，比如版本号是cadab353589f3eef075817b890dafe8b722d802b，  
那么就可以直接使用命令：  
$ git reset --hard cadab353589f            使用前几位表示即可，git会自动查找  
注：版本回退以后，使用git log打印的历史记录都是回退版本之前的数据，之后的都没有了，不过放心，git总有后悔药可以吃哒~  
1.如果命令行窗口没有关闭，直接去前面找commit id即可；  
2.如果命令行窗口关闭了，或者第二天后悔了，可以进入到该目录下，使用git reflog命令来查看以前的每一次命令，可以获得每次提交的commit id，  
就可以版本回退了。  
$ git reflog                           可以查看命令历史，包含提交的commit id  
```

**版本回退原理**

![版本回退](http://i.imgur.com/4SgX3lI.png)

简单讲，就是说只要进行了代码提交，git内部都会按照时间节点进行记录，每条记录都有commit id作为唯一标识(就像是链表每个节点都有唯一的地址一样)，HEAD总是指向当前版本(就像指针一样)。所谓的版本回退，仅仅是讲Head从当前版本指向了指定的版本，然后将工作区的文件也修改了。

**工作区和暂存区**

    Git和其他版本控制系统的一个不同之处就是有暂存区的概念。
    - 工作区
    就是电脑里能看到的目录，比如上面创建的C:\Android\git-repositories\new_repository_1文件夹就是一个工作区。
    - 版本库
    工作区中有一个隐藏目录.git，就是Git的版本库，版本库里存放了很多的东西，其中最重要的就是state(或者叫index)的暂存区，  
    还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
    
 > 前面讲到，将文件存入到Git版本库里，分两步执行：
    
    第一步：用git add命令将工作区的修改文件添加到暂存区；  （多次操作）
    
![第一步](http://i.imgur.com/S0pYWlQ.png)

    第二步：用git commit命令将暂存区的所有修改内容提交到当前分支。（事务提交，包含第一步多次操作，注意，不在暂存区的修改不会被commit） 
    一旦事务提交之后，如果对工作区没有做什么修改，那么工作区就是干净的。 
    因为创建Git版本库的时候，Git自动创建了一个master分支，所以现在git commit 就是往master分支上提交事务。 
    Eg： 
