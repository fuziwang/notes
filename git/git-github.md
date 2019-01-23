## GitHub

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

### 提交代码到远程仓库

现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。

首先，登陆GitHub，然后，在右上角找到`“new repository”`按钮，创建一个新的仓库：

![创建仓库](https://upload-images.jianshu.io/upload_images/12817540-49e071d6edeabc0e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在Repository name填入`learngit`，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：

![创建仓库](https://upload-images.jianshu.io/upload_images/12817540-b3f108f310f6acd2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

目前，在GitHub上的这个`learngit`仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

现在，我们根据GitHub的提示，在本地的`learngit`仓库下运行命令：

```bash
~/learngit(master) » git remote add origin https://github.com/fuziwang/learngit.git
```

添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

下一步，就可以把本地库的所有内容推送到远程库上：

```bash
~/learngit(master) » git push -u origin master                   
Counting objects: 43, done.
Compressing objects: 100% (38/38), done.
Writing objects: 100% (43/43), 3.57 KiB | 0 bytes/s, done.
Total 43 (delta 14), reused 0 (delta 0)
remote: Resolving deltas: 100% (14/14), done.
To https://github.com/fuziwang/learngit.git
 * [new branch]      master -> master
分支 master 设置为跟踪来自 origin 的远程分支 master。
```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

![本地上传到远程](https://upload-images.jianshu.io/upload_images/12817540-7fab366f7dfc20c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从现在起，只要本地作了提交，就可以通过命令：

```bash
~/learngit(master) » git push origin master
```

把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

**原理图如下：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190123211213439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Z1eml3YW5n,size_16,color_FFFFFF,t_70)

### 将远程仓库代码更新到本地

首先我们新建一文件夹：learngit，进入该文件夹后使用git 指令：

```bash
git clone https://github.com/fuziwang/learngit
```

指令执行完毕后， 就在该文件夹下生成一份副本啦（相当于多人协作时另一台设备上的工程文件），**原理图如下：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190123211200696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Z1eml3YW5n,size_16,color_FFFFFF,t_70)

接下来， 讨论`git pull`、 `git fetch` 、 `git merge`的关系：

+ `git fetch` 相当于是从远程获取最新到本地，不会自动`merge` 
+ `git pull`：相当于是从远程获取最新版本并`merge`到本地 在实际使用中，`git fetch`更安全一些

### 更新到本地仓库时解决冲突

有两种方式处理冲突: 放弃本地修改或 解决冲突后提交本地修改

+ 放弃本地修改

放弃本地修改意味着将远程仓库的代码完全覆盖本地仓库以及本地工作区间， 如果对git的指令不熟悉那大可以将本地工程完全删除，然后再重新拷贝一次（`git clone`）。

当然， git如此强大没必要用这么原始的方法，可以让本地仓库代码覆盖本地修改，然后更新远程仓库代码； 

本地仓库代码完全覆盖本地工作区间，具体指令如下：

```bash
git checkout HEAD .
```

然后更新远程仓库的代码就不会出现冲突了。

+ 解决冲突后提交本地修改

解决冲突后提交本地修改的思路大概如下：

将本地修改的代码放在缓存区， 然后从远程仓库拉取最新代码，拉取成功后再从缓存区将修改的代码取出， 这样最新代码跟本地修改的代码就会混杂在一起， 手工解决冲突后， 提交解决冲突后的代码。

将本地修改放入缓存区(成功后本地工作区间的代码跟本地仓库代码会同步)， 具体指令：

```bash
git stash
```

从远程仓库获取最新代码，然后， 取出本地修改的代码， 具体指令：

```bash
git pull
git stash pop
```

随后手工解决冲突即可！

## Git图解

### 基本用法

![](https://images0.cnblogs.com/i/66979/201406/262308046017546.jpg)

上面的四条命令在工作目录、暂存目录(也叫做索引)和仓库之间复制文件。

- `git add files` 把当前文件放入暂存区域。
- `git commit` 给暂存区域生成快照并提交。
- `git reset -- files` 用来撤销最后一次`git add files`，你也可以用`git reset` 撤销所有暂存区域文件。（操作对象是HEAD）
- `git checkout -- files` 把文件从暂存区域复制到工作目录，用来丢弃本地修改。（目的是working Directory）

![](https://images0.cnblogs.com/i/66979/201406/262311265392839.jpg)

### 工作区、版本库、暂存区

![](https://images0.cnblogs.com/blog/66979/201412/191735587664863.png)

### Git文件状态

![](https://images0.cnblogs.com/blog/168097/201308/07173755-e9cd02568038429e9546269a231adc94.png)

在工作目录中的文件被分为两种状态，一种是已跟踪状态(tracked)，另一种是未跟踪状态(untracked)。只有处于已跟踪状态的文件才被纳入GIT的版本控制。