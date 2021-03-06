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
### Feature分支

添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

现在，你终于接到了一个新任务：开发代号为Vulcan的新功能，该功能计划用于下一代星际飞船。

```bash
~/learngit(dev) » git checkout -b feature-vulcan                 
切换到一个新分支 'feature-vulcan'
```

5分钟后，开发完毕：

```bash
~/learngit(feature-vulcan*) » git add vulcan.js                  
------------------------------------------------------------
~/learngit(feature-vulcan*) » git commit -m "add feature vulcan" 
[feature-vulcan 65d109d] add feature vulcan
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 vulcan.js
```

切回`dev`，准备合并：

```bash
~/learngit(feature-vulcan) » git checkout dev                    
切换到分支 'dev'
```

一切顺利的话，feature分支和bug分支是类似的，合并，然后删除。

就在此时，接到上级命令，因经费不足，新功能必须取消！虽然白干了，但是这个包含机密资料的分支还是必须就地销毁：

```bash
~/learngit(dev) » git branch -d feature-vulcan                   
error: 分支 'feature-vulcan' 没有完全合并。
如果您确认要删除它，执行 'git branch -D feature-vulcan'。
```

销毁失败。Git友情提醒，`feature-vulcan`分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的`-D`参数。现在我们强行删除：

```
~/learngit(dev) » git branch -D feature-vulcan                   
已删除分支 feature-vulcan（曾为 65d109d）。
```

总结：

+ 开发一个新feature，最好新建一个分支；


+ 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D `强行删除。

### 多人协作

当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用`git remote`，或者，用`git remote -v`显示更详细的信息：

```bash
~/learngit(master) » git remote                                  
origin
------------------------------------------------------------
~/learngit(master) » git remote -v                               
origin	https://github.com/fuziwang/learngit.git (fetch)
origin	https://github.com/fuziwang/learngit.git (push)
```

上面显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到push的地址。

#### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```bash
~/learngit(master) » git push origin master
```

如果要推送其他分支，比如`dev`，就改成：

```bash
~/learngit(master) » git push origin dev
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！

因此，多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin `推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin `推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to  origin/`。

> 更多的多人协作的功能在`git-flow.md`上展示

## 参考资料

- [猴子都能懂的GIT入门](http://backlogtool.com/git-guide/cn/)


- [Git教程 - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
- 非常感谢王顶老师提供的centos的Linux系统环境