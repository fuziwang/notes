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

