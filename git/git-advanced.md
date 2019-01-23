## Git 与 GitHub

### 远程仓库

Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。怎么分布呢？最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分。

有个叫`GitHub`这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。

你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```bash
~ » ssh-keygen -t rsa -C "2622860598@qq.com"                     
Generating public/private rsa key pair.
Enter file in which to save the key (/home/wangding/.ssh/id_rsa): 
Created directory '/home/wangding/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/wangding/.ssh/id_rsa.
Your public key has been saved in /home/wangding/.ssh/id_rsa.pub.
The key fingerprint is:
ac:96:f7:3a:aa:01:16:dd:9f:19:f2:97:ed:3c:5e:60 2622860598@qq.com
The key's randomart image is:
+--[ RSA 2048]----+
|                 |
|   . .           |
|  . . o .        |
|   .   = + o     |
|  o     S o E    |
| . .   o . + .   |
|    . + .   + .  |
|     o ... . o   |
|    .....o. .    |
+-----------------+
```

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

![ssh](https://upload-images.jianshu.io/upload_images/12817540-bfc9c045cd7161b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点“Add Key”，你就应该看到已经添加的Key：

**为什么GitHub需要SSH Key呢？**因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

### 添加远程库

现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。

首先，登陆GitHub，然后，在右上角找到`“new repository”`按钮，创建一个新的仓库：

![创建仓库](https://upload-images.jianshu.io/upload_images/12817540-49e071d6edeabc0e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在Repository name填入`learngit`，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：

![创建仓库](https://upload-images.jianshu.io/upload_images/12817540-b3f108f310f6acd2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

目前，在GitHub上的这个`learngit`仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

现在，我们根据GitHub的提示，在本地的`learngit`仓库下运行命令：

```bash
$ git remote add origin git@github.com:fuziwang/learngit.git
```

请千万注意，把上面的`fuziwang`替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。

添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

下一步，就可以把本地库的所有内容推送到远程库上：

```bash
$ git push -u origin master
Counting objects: 20, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (15/15), done.
Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.
Total 20 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), done.
To github.com:michaelliao/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

![本地上传到远程](https://upload-images.jianshu.io/upload_images/12817540-7fab366f7dfc20c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从现在起，只要本地作了提交，就可以通过命令：

```bash
$ git push origin master
```

把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

### 从远程库克隆

上次我们讲了先有本地库，后有远程库的时候，如何关联远程库。

现在，假设我们已经创建了远程库，然后，从远程库克隆。

![克隆](https://upload-images.jianshu.io/upload_images/12817540-bf13da38d9a0f76f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在，远程库已经准备好了，下一步是用命令`git clone`克隆一个本地库：

```bash
~ » git clone git@github.com:fuziwang/learngit.git
Cloning into 'gitskills'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
Receiving objects: 100% (3/3), done.
```

注意把Git库的地址换成你自己的，然后进入`learngit`目录看看，已经有上述的文件了：

```bash
~ » cd learngit                                                  
------------------------------------------------------------
~/learngit(master) » ls                                          
LICENSE  readme.txt
```

如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

你也许还注意到，GitHub给出的地址不止一个，还可以用`https://github.com/fuziwang/learngit.git`这样的地址。实际上，Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。

## 分支管理

分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

### 创建与合并分支

在[版本回退](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013744142037508cf42e51debf49668810645e02887691000)里，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

![git-br-initial](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849087937492135fbf4bbd24dfcbc18349a8a59d36d000/0)

每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长：

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

![git-br-create](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0)

从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

![git-br-dev-fd](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849088235627813efe7649b4f008900e5365bb72323000/0)

假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：

![git-br-ff-merge](https://cdn.liaoxuefeng.com/cdn/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0)

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

![git-br-rm](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908867187c83ca970bf0f46efa19badad99c40235000/0)

**下面开始实战。**

首先，我们创建`dev`分支，然后切换到`dev`分支：

```bash
~/learngit(master) » git checkout -b dev                         
切换到一个新分支 'dev'
```

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```bash
~/learngit(master) » git branch dev
创建到一个新分支 'dev'
~/learngit(master) » git checkout dev
切换到一个新分支 'dev'
```

然后，用`git branch`命令查看当前分支：

```bash
~/learngit(dev) » git branch                                     
* dev
  master
```

`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。

然后，我们就可以在`dev`分支上正常提交，比如对readme.txt做个修改，加上一行：

```txt
Creating a new branch is quick.
```

然后提交：

```bash
~/learngit(dev*) » git add readme.txt                            
------------------------------------------------------------
~/learngit(dev*) » git commit -m "branch test"                   
[dev faa2e24] branch test
 1 file changed, 1 insertion(+)
```

现在，`dev`分支的工作完成，我们就可以切换回`master`分支：

```bash
~/learngit(dev) » git checkout master                            
切换到分支 'master'
```

切换回`master`分支后，再查看一个readme.txt文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：

![git-br-on-master](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908892295909f96758654469cad60dc50edfa9abd000/0)

现在，我们把`dev`分支的工作成果合并到`master`分支上：

```bash
~/learngit(master) » git merge dev                               
更新 87df41b..faa2e24
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

`git merge`命令用于合并指定分支到当前分支。合并后，再查看readme.txt的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

合并完成后，就可以放心地删除`dev`分支了，删除后，查看`branch`，就只剩下`master`分支了：

```bash
~/learngit(master) » git branch -d dev                           
已删除分支 dev（曾为 faa2e24）。
------------------------------------------------------------
~/learngit(master) » git branch                                  
* master
```

因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

### 解决冲突

准备新的`feature1`分支，继续我们的新分支开发：

```bash
~/learngit(master) » git checkout -b feature                     
切换到一个新分支 'feature'
```

修改`readme.txt`最后一行，改为：

```txt
Creating a new branch is quick AND simple.
```

在`feature1`分支上提交，切换到`master`分支：

```bash
~/learngit(feature*) » git add readme.txt                        
------------------------------------------------------------
~/learngit(feature*) » git commit -m "AND simple"                
[feature a8b263e] AND simple
 1 file changed, 1 insertion(+), 1 deletion(-)
------------------------------------------------------------
~/learngit(feature) » git checkout master                        
切换到分支 'master'
```

在`master`分支上把`readme.txt`文件的最后一行改为：

```txt
Creating a new branch is quick & simple.
```

提交：

```bash
~/learngit(master*) » git add readme.txt                         
------------------------------------------------------------
~/learngit(master*) » git commit -m "& simple"                   
[master 5565d9c] & simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

现在，`master`分支和`feature1`分支各自都分别有新的提交，变成了这样：

![git-br-feature1](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909115478645b93e2b5ae4dc78da049a0d1704a41000/0)

这种情况下，Git无法执行“快速合并”，**只能试图把各自的修改合并起来，但这种合并就可能会有冲突**：

```bash
~/learngit(master) » git merge feature                           
自动合并 readme.txt
冲突（内容）：合并冲突于 readme.txt
自动合并失败，修正冲突然后提交修正的结果。
```

Git告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件：

```bash
~/learngit(master*) » git status                                 
# 位于分支 master
# 您有尚未合并的路径。
#   （解决冲突并运行 "git commit"）
#
# 未合并的路径：
#   （使用 "git add <file>..." 标记解决方案）
#
#	双方修改：     readme.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

我们可以直接查看readme.txt的内容：

```bash
~/learngit(master*) » cat readme.txt                             
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature
```

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存，再提交：

```bash
~/learngit(master*) » cat readme.txt                             
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick and simple.
------------------------------------------------------------
~/learngit(master*) » git add readme.txt                         
------------------------------------------------------------
~/learngit(master*) » git commit -m "conflict fixed"             
[master a0c8a45] conflict fixed
```

现在，`master`分支和`feature1`分支变成了下图所示：

![git-br-conflict-merged](https://cdn.liaoxuefeng.com/cdn/files/attachments/00138490913052149c4b2cd9702422aa387ac024943921b000/0)

用带参数的`git log`也可以看到分支的合并情况，最后，删除`feature1`分支：

```bash
~/learngit(master) » git log --graph --pretty=oneline --abbrev-commit
*   a0c8a45 conflict fixed
|\  
| * a8b263e AND simple
* | 5565d9c & simple
|/  
* faa2e24 branch test
...
------------------------------------------------------------
~/learngit(master) » git branch -d feature                       
已删除分支 feature（曾为 a8b263e）。
```

总结：

+ 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。


+ 解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。


+ 用`git log --graph`命令可以看到分支合并图。

### 分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

+ 合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

下面我们实战一下`--no-ff`方式的`git merge`：

首先，仍然创建并切换`dev`分支，修改readme.txt文件，并提交一个新的commit：

```bash
~/learngit(master) » git checkout -b dev                         
切换到一个新分支 'dev'
------------------------------------------------------------
~/learngit(dev) » vi readme.txt                                  
------------------------------------------------------------
~/learngit(dev*) » git add readme.txt                            
------------------------------------------------------------
~/learngit(dev*) » git commit -m "add merge"                     
[dev ecca429] add merge
 1 file changed, 1 insertion(+)
```

现在，我们切换回`master`，准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

```bash
~/learngit(dev) » git checkout master                            
切换到分支 'master'
------------------------------------------------------------
~/learngit(master) » git merge --no-ff -m "merge with no-ff" dev 
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

合并后，我们用`git log`看看分支历史：

```bash
~/learngit(master) » git log --graph --pretty=oneline --abbrev-commit
*   fc2593c merge with no-ff
|\  
| * ecca429 add merge
|/  
*   2a45671 conflict fixed
...
```

可以看到，不使用`Fast forward`模式，merge后就像这样：

![git-no-ff-mode](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909222841acf964ec9e6a4629a35a7a30588281bb000/0)

#### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![git-br-policy](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)

### Bug分支

软件开发中，有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交：

```bash
~/learngit(dev*) » git status                                    
# 位于分支 dev
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	新文件：    test.js
```

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

幸好，Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```bash
~/learngit(dev*) » git stash                                     
Saved working directory and index state WIP on dev: ecca429 add merge
HEAD 现在位于 ecca429 add merge
```

现在，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

```bash
~/learngit(dev) » git checkout master                            
切换到分支 'master'
------------------------------------------------------------
~/learngit(master) » git checkout -b issue-101                   
切换到一个新分支 'issue-101'
```

现在修复bug，需要把“Git is free software ...”改为“Git is a free software ...”，然后提交：

```bash
~/learngit(issue-101*) » git add readme.txt                      
------------------------------------------------------------
~/learngit(issue-101*) » git commit -m "fix bug 101"             
[issue-101 c6742f9] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

```bash
~/learngit(issue-101) » git checkout master                      
切换到分支 'master'
------------------------------------------------------------
~/learngit(master) » git merge --no-ff -m "merge bug fix 101" issue-101 
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

太棒了，原计划两个小时的bug修复只花了5分钟！现在，是时候接着回到`dev`分支干活了！工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```bash
~/learngit(master) » git checkout dev                            
切换到分支 'dev'
------------------------------------------------------------
~/learngit(dev) » git status                                     
# 位于分支 dev
无文件要提交，干净的工作区
------------------------------------------------------------
~/learngit(dev) » git stash list                                 
stash@{0}: WIP on dev: ecca429 add merge
```

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

+ 用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；


+ 是用`git stash pop`，恢复的同时把stash内容也删了：

```bash
~/learngit(dev) » git stash pop                                  
# 位于分支 dev
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	新文件：    test.js
#
丢弃了 refs/stash@{0} (9f5aa291757ed26ad33bc6c7931e4c85c71d1cc0)
```

再用`git stash list`查看，就看不到任何stash内容了，你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```bash
~/learngit(dev) » git stash apply stash@{0}
```