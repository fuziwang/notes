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

### 监视文件变动

我们已经成功地添加并提交了一个`readme.txt`文件，现在，是时候继续工作了，于是，我们继续修改readme.txt文件，改成如下内容：

```txt
Git is a distributed version control system.
Git is free software.
```

现在，运行`git status`命令看看结果：

```bash
~/learngit(master*) » git status                                 
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      readme.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

`git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，`readme.txt`被修改过了，但还没有准备提交的修改。

虽然Git告诉我们`readme.txt`被修改了，但如果能看看具体修改了什么内容，需要用`git diff`这个命令看看：

```bash
~/learngit(master*) » git diff readme.txt                       
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

`git diff`顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，可以从上面的命令输出看到，我们在第一行添加了一个`distributed`单词。

知道了对`readme.txt`作了什么修改后，再把它提交到仓库就放心多了，**提交修改和提交新文件是一样的两步**，第一步是`git add`：

```bash
~/learngit(master*) » git add readme.txt                        
```

同样没有任何输出。在执行第二步`git commit`之前，我们再运行`git status`看看当前仓库的状态：

```bash
~/learngit(master*) » git status                                 wangding@OFFICE
# 位于分支 master
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	修改：      readme.txt
#
```

`git status`告诉我们，将要被提交的修改包括`readme.txt`，下一步，就可以放心地提交了：

```bash
~/learngit(master*) » git commit -m "add distributed"           
[master e90d7f4] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)
```

提交后，我们再用`git status`命令看看仓库的当前状态：

```bash
~/learngit(master) » git status                                  
# 位于分支 master
无文件要提交，干净的工作区
```

Git告诉我们当前没有需要提交的修改，而且，工作目录是干净`（working tree clean）`的。

### 版本回退

Git除了可以进行修改，还可以进行版本的回退，每当你觉得文件修改到一定程度的时候，就可以**“保存一个快照”**，这个快照在Git中被称为`commit`。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用`git log`命令查看：

```bash
~/learngit(master) » git log                                    
commit 3f1f286a28dafdfdba3e03eca3cc6f10f1e7e9ce
Author: fuziwang <2622860598@qq.com>
Date:   Tue Jan 22 13:48:26 2019 +0800

    append GPL

commit e90d7f425f63361ba89efe590de4edf3e422c02e
Author: fuziwang <2622860598@qq.com>
Date:   Tue Jan 22 13:39:40 2019 +0800

    add distributed

commit cfe0428347efabaa78f8dad416c8e56f72b90fd1
Author: fuziwang <2622860598@qq.com>
Date:   Mon Jan 21 23:04:10 2019 +0800

    wrote a readme file
```

`git log`命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是`append GPL`，上一次是`add distributed`，最早的一次是`wrote a readme file`。

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

```bash
~/learngit(master) » git log --pretty=oneline                    
3f1f286a28dafdfdba3e03eca3cc6f10f1e7e9ce append GPL
e90d7f425f63361ba89efe590de4edf3e422c02e add distributed
cfe0428347efabaa78f8dad416c8e56f72b90fd1 wrote a readme file
```

需要友情提示的是，你看到的一大串类似`3f1f286...`的是`commit id`（版本号）

**准备把readme.txt回退到上一个版本，也就是add distributed的那个版本，怎么做呢？**

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

现在，我们要把当前版本`append GPL`回退到上一个版本`add distributed`，就可以使用`git reset`命令：

```bash
~/learngit(master) » git reset --hard HEAD^                      
HEAD 现在位于 e90d7f4 add distributed
```

看看`readme.txt`的内容是不是版本`add distributed`：

```bash
~/learngit(master) » cat readme.txt
Git is a distributed version control system.
Git is free software.
```

还可以继续回退到上一个版本`wrote a readme file`，不过且慢，然我们用`git log`再看看现在版本库的状态：

```bash
~/learngit(master) » git log                                     
commit e90d7f425f63361ba89efe590de4edf3e422c02e
Author: fuziwang <2622860598@qq.com>
Date:   Tue Jan 22 13:39:40 2019 +0800

    add distributed

commit cfe0428347efabaa78f8dad416c8e56f72b90fd1
Author: fuziwang <2622860598@qq.com>
Date:   Mon Jan 21 23:04:10 2019 +0800

    wrote a readme file
```

最新的那个版本`append GPL`已经看不到了！如何再恢复撤销的版本？只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个`append GPL`的`commit id`是`3f1f286...`，于是就可以指定回到未来的某个版本：

```bash
~/learngit(master) » git reset --hard 3f1f286                    
HEAD 现在位于 3f1f286 append GPL
```

版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

再小心翼翼地看看`readme.txt`的内容：

```bash
~/learngit(master) » cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把HEAD从指向`append GPL`：

![git版本回退](https://upload-images.jianshu.io/upload_images/12817540-8a35ad6dc8ff7228.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

改为指向`add distributed`：

![git版本回退](https://upload-images.jianshu.io/upload_images/12817540-25e56242243fab92.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后顺便把工作区的文件更新了。所以你让`HEAD`指向哪个版本号，你就把当前版本定位在哪。

现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的`commit id`怎么办？Git提供了一个命令`git reflog`用来记录你的每一次命令：

```bash
~/learngit(master) » git reflog
3f1f286 HEAD@{0}: reset: moving to 3f1f286
e90d7f4 HEAD@{1}: reset: moving to HEAD^
3f1f286 HEAD@{2}: commit: append GPL
e90d7f4 HEAD@{3}: commit: add distributed
cfe0428 HEAD@{4}: commit (initial): wrote a readme file
```

从输出可知，`append GPL`的commit id是`3f1f286`，现在，你又可以根据新版本的id进行版本的回退

### 继续理解暂存区

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，**实际上就是把文件修改添加到暂存区；**

第二步是用`git commit`提交更改，**实际上就是把暂存区的所有内容提交到当前分支。**

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在`git commit`就是往`master`分支上提交更改。

**你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。**

现在，我们再练习一遍，先对`readme.txt`做个修改，比如加上一行内容：

```txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
```

然后，在工作区新增一个`LICENSE`文本文件（内容随便写）。

先用`git status`查看一下状态：

```bash
~/learngit(master*) » git status                                 
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      readme.txt
#
# 未跟踪的文件:
#   （使用 "git add <file>..." 以包含要提交的内容）
#
#	LICENSE
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

Git非常清楚地告诉我们，`readme.txt`被修改了，而`LICENSE`还从来没有被添加过，所以它的状态是`Untracked`。

现在，使用两次命令`git add`，把`readme.txt`和`LICENSE`都添加后，用`git status`再查看一下：

```bash
~/learngit(master*) » git add -A                                 
------------------------------------------------------------
~/learngit(master*) » git status                                 
# 位于分支 master
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	新文件：    LICENSE
#	修改：      readme.txt
```

现在，暂存区的状态就变成这样了：

![暂存区改变](https://upload-images.jianshu.io/upload_images/12817540-aec0306e64565760.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以，`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

```bash
~/learngit(master*) » git commit -m "understand how stage works"
[master 6d8e266] understand how stage works
 2 files changed, 2 insertions(+)
 create mode 100644 LICENSE
```

一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：

```bash
~/learngit(master) » git status                                  
# 位于分支 master
无文件要提交，干净的工作区
```

现在版本库变成了这样，暂存区就没有任何内容了：

![暂存区commit之后](https://upload-images.jianshu.io/upload_images/12817540-aaa56b67c202baa1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 管理修改

为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。（所谓修改就是对文件的增、删、改、创建等更新操作）

为什么说Git管理的是修改，而不是文件呢？

第一步，对readme.txt做一个修改，比如加一行内容：

```bash
~/learngit(master*) » cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
```

然后，添加：

```bash
~/learngit(master*) » git add readme.txt                         
------------------------------------------------------------
~/learngit(master*) » git status                                 
# 位于分支 master
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	修改：      readme.txt
```

然后，再修改readme.txt：

```bash
~/learngit(master*) » cat readme.txt 
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
```

提交：

```bash
~/learngit(master*) » git commit -m "git tracks changes"         
[master 8bcc0ba] git tracks changes
 1 file changed, 1 insertion(+)
```

提交后，再看看状态：

```bash
~/learngit(master*) ? git status                                 
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      readme.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

我们发现，第二次的修改没有被提交，我们回顾一下操作过程：

第一次修改 -> `git add` -> 第二次修改 -> `git commit`

Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

提交后，用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别：

```bash
~/learngit(master*) » git diff HEAD -- readme.txt                
diff --git a/readme.txt b/readme.txt
index 76d770f..a9c5755 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,4 +1,4 @@
 Git is a distributed version control system.
 Git is free software distributed under the GPL.
 Git has a mutable index called stage.
-Git tracks changes.
+Git tracks changes of files.
```

可见，第二次修改确实没有被提交。

那怎么提交第二次修改呢？你可以继续`git add`再`git commit`，也可以别着急提交第一次修改，先`git add`第二次修改，再`git commit`，就相当于把两次修改合并后一块提交了：

第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

### 撤销修改

现在，你在`readme.txt`中添加了一行：

```bash
~/learngit(master*) » cat readme.txt                             
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN.
```

你可以删掉最后一行，手动把文件恢复到上一个版本的状态。如果用`git status`查看一下：

```
~/learngit(master*) » git status                                 
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      readme.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

你可以发现，Git会告诉你，`git checkout -- file`可以丢弃工作区的修改：

```bash
~/learngit(master*) » git checkout -- readme.txt
```

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

**总之，就是让这个文件回到最近一次git commit或git add时的状态。**

现在，看看`readme.txt`的文件内容：

```bash
~/learngit(master) » cat readme.txt                              
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
```

文件内容果然复原了。

现在，你不但写了一些胡话，还`git add`到暂存区了：

```bash
~/learngit(master*) » cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN.

~/learngit(master*) » git add readme.txt
```

庆幸的是，在`commit`之前，你发现了这个问题。用`git status`查看一下，修改只是添加到了暂存区，还没有提交：

```bash
~/learngit(master*) » git status                                 
# 位于分支 master
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	修改：      readme.txt
```

Git同样告诉我们，用命令`git reset HEAD`可以把暂存区的修改撤销掉（unstage），重新放回工作区：

```bash
~/learngit(master*) » git reset HEAD readme.txt                  
重置后撤出暂存区的变更：
M	readme.txt
```

`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。

再用`git status`查看一下，现在暂存区是干净的，工作区有修改：

```bash
~/learngit(master*) » git status                                 
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      readme.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

还记得如何丢弃工作区的修改吗？

```bash
~/learngit(master*) » git checkout -- readme.txt   
```

![撤销修改](https://upload-images.jianshu.io/upload_images/12817540-8a82af1818f654c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 删除文件

在Git中，删除也是一个修改操作，先添加一个新文件`test.txt`到Git并且提交：

```bash
~/learngit(master*) » git add test.txt                          
------------------------------------------------------------
~/learngit(master*) » git commit -m "add test.txt"               
[master 5100a61] add test.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test.txt
```

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：

```bash
~/learngit(master) » rm test.txt                                 
```

这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了：

```bash
~/learngit(master*) » git status                                 
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add/rm <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	删除：      test.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

```bash
~/learngit(master*) » git rm test.txt
rm 'test.txt'
------------------------------------------------------------
~/learngit(master*) » git commit -m "remove test.txt"
[master 87df41b] remove test.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 test.txt
```

现在，文件就从版本库中被删除了。

**小提示：**先手动删除文件，然后使用`git rm `和`git add`效果是一样的。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```bash
~/learngit(master*) » git checkout -- test.txt
```

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

## 参考资料

+ [猴子都能懂的GIT入门](http://backlogtool.com/git-guide/cn/)


+ [Git教程 - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
+ 非常感谢王顶老师提供的centos的Linux系统环境