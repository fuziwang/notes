## 首先

首先提出一个问题：要把文档还原到编辑前的状态，大家都是怎么做的呢？

最简单的方法就是先备份编辑前的文档。使用这个方法时，我们通常都会在备份的文档名或目录名上添加编辑的日期。

![](https://backlog.com/git-tutorial/cn/img/post/intro/capture_intro1_1_1.png)

上述的方法有什么缺点？

+ 每次编辑文档都要事先复制，这样非常麻烦，也很容易出错。
+ 如果像上图那样毫无命名规则的话，就无法区分哪一个文档是最新的了。
+ 那些文档名字没有体现修改内容。并不能知道文章到底修改了哪些地方
+ 如果两个人同时编辑某个共享文件，先进行编辑的人所做的修改内容会被覆盖，相信大家都有这样的经历。（不利于多人协作）

Git版本管理系统就是为了解决这些问题应运而生的。

## git简介

Git是一个分布式版本管理系统，是为了更好地管理Linux内核开发而创立的。

Git可以在任何时间点，把文档的状态作为更新记录保存起来。因此可以把编辑过的文档复原到以前的状态，也可以显示编辑前后的内容差异。

而且，编辑旧文件后，试图覆盖较新的文件的时候（即上传文件到服务器时），系统会发出警告，因此可以避免在无意中覆盖了他人的编辑内容。

### 关于版本控制

**版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。**

- 版本控制可以将某一个文件回溯到以前的状态,甚至将整个项目都回退到过去某个时间点的状态


- 可以比较文件的变化细节,查出最后是谁修改了哪个地方,从而找出导致怪异问题出现的原因，又是谁在何时报告了某个功能缺陷

#### 本地版本控制

本地版本控制系统：采用某种简单的数据库来记录文件的历次更新差异

![本地版本控制](https://upload-images.jianshu.io/upload_images/12817540-5ce53cfd0e70ed54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 集中化的版本控制系统

集中化的版本控制系统（Centralized Version Control Systems，简称 CVCS）都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新

- 好处：每个人都可以在一定程度上看到项目中的其他人正在做些什么， 而管理员也可以轻松掌控每个开发者的权限，并且管理一个 CVCS 要远比在各个客户端上维护本地数据库来得轻松容易**（即实现了不同开发者的协同的工作）**


- 不足：如果中央服务器发生故障，谁都无法提交更新，也就无法协同工作；如果中心数据库所在的磁盘发生损坏，又没有做恰当备份，将丢失所有数据——包括项目的整个变更历史，只剩下人们在各自机器上保留的单独快照**（集中化可能导致中心数据库丢失或者中心服务器发生故障）**

![集中化版本控制](https://upload-images.jianshu.io/upload_images/12817540-4a24084df74bb9ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 分布式版本控制系统

分布式版本控制系统（Distributed Version Control System，简称 DVCS）是指客户端并不只提取最新版本的文件快照，而是**把代码仓库完整地镜像下来** 。常见的分布式版本控制系统有：Git、Mercurial、Bazaar、Darcs 等

- 任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。 因为每一次的克隆操作，实际上都是一次对代码仓库的完整备份。


- 许多这类系统都可以指定和若干不同的远端代码仓库进行交互。籍此，你就可以在同一个项目中，分别和不同工作小组的人相互协作。 你可以根据需要设定不同的协作流程，比如层次模型式的工作流，而这在以前的集中式系统中是无法实现的。

![分布式版本控制](https://upload-images.jianshu.io/upload_images/12817540-9571bb42eebbcdbf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 安装git

**在Linux上安装Git**

+ Debian或Ubuntu Linux，通过`sudo apt-get install git`就可以直接完成Git的安装


+ 如果是其他Linux版本，可以直接通过源码安装。先从Git官网下载源码，然后解压，依次输入：`./config`，`make`，`sudo make install`这几个命令安装就好了。

## 工作区和暂存区

Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。

+ **工作区**（Working Directory）就是你在电脑里能看到的目录


+ **版本库**（Repository）工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![版本库](https://upload-images.jianshu.io/upload_images/12817540-b5e52f8e4fbf41eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### git初始配置

- 检验git安装是否成功：git bash中运行 `git --version`验证安装是否成功

**设置git参数**

- 显示当前的git配置：`git config --list`
- 设置提交仓库时的用户名信息：`git config --global user.name "username"`
- 设置提交仓库时的邮箱信息：`git config --global user.email "email"`

### 创建版本库

什么是版本库呢？版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

#### 创建一个版本库的步骤

首先，选择一个合适的地方，创建一个空目录：

```bash
~ » mkdir learngit                                                                               
------------------------------------------------------------
~ » cd learngit                                                                                  
------------------------------------------------------------
~/learngit » pwd                                                                                  
/home/wangding/learngit
```

第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：

```bash
~/learngit » git init                                                                            
初始化空的 Git 版本库于 /home/wangding/learngit/.git/
```

瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

第三步，把文件添加到版本库

现在我们编写一个`readme.txt`文件，内容如下：

```markdown
Git is a version control system.
Git is free software.
```

#### 文件添加到git仓库的步骤

第一步，用命令`git add`告诉Git，把文件添加到仓库：

```bash
~/learngit(master*) » git add readme.txt                                                          
```

执行上面的命令，没有任何显示，说明添加成功。

第二步，用命令`git commit`告诉Git，把文件提交到仓库：

```bash
~/learngit(master*) » git commit -m "wrote a readme file"                                       
[master（根提交） cfe0428] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

`git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt有两行内容）。

## 参考资料

+ [猴子都能懂的GIT入门](http://backlogtool.com/git-guide/cn/)


+ [Git教程 - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
+ 非常感谢王顶老师提供的centos的Linux系统环境