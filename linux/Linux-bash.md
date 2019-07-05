## Linux介绍

Linux是一个开源的操作系统内核，发布于GPL协议，全称是GNU/Linux。

- **Linux和Unix是不同的**，但它们看起来很像。因为Linux参考了Unix的设计思想。
- Linux简洁、高效、功能强大。

Linux的开发者是**Linus**（林纳斯·托瓦兹）

查看自己电脑的系统的发行版：

```bash
frewen@ubuntu:~$ uname -a
版本：Ubuntu 16.04
内核：GNU/Linux
发布协议：GPL协议(类似的协议有Apache MIT)
文件系统：Ext4
```

Linux上使用shutdown命令进行关机重启，定时关机等操作。

实际上Windows启动cmd或者是PowerShell运行shutdown也是可以的，当点击关机的时候调用的就是`shutdown`。

```bash
# 关机命令
frewen@ubuntu:~$ sudo shutdown -h now
# 重启命令
frewen@ubuntu:~$ sudo shutdown -r now
```

## 目录结构与磁盘管理

Linux没有“盘符”的概念，Windows会分为C盘D盘等。而Linux通过一个整体的**目录树**来组织文件。

Linux使用 **/** 表示根目录，也就是整个目录树的顶层。其他的目录都位于**/**之下。

**Linux启动时会把磁盘存储的文件信息映射为内存中的树型结构**。我们在操作中用到的目录结构，是建立在内存中的目录结构，要与磁盘文件系统中的目录结构区分开。

所有的目录都至少包含两个子目录，**. 和 .. ，. 表示当前目录，.. 表示上一层目录。/也有 .. ，但是指向的是自己。**

```bash
# 展示当前的目录
frewen@ubuntu:~$ pwd
/home/frewen
```

|    目录     | 说明                                                         |
| :---------: | :----------------------------------------------------------- |
|      /      | 系统根目录                                                   |
|    /usr     | 用户的程序，配置等信息都放在这个目录下                       |
|    /bin     | 存放常用命令的目录，此目录和/sbin目录存放系统**最核心的命令** |
|    /home    | 主目录，所有用户主目录都会在此目录下，以用户名命名           |
|    /sbin    | 超级用户root才能使用的命令所在的目录                         |
|    /lib     | 系统动态链接共享库，类似于Windows下的dll文件所在的目录       |
|    /boot    | 系统启动文件所在目录，如GRUB、内核、initrd等                 |
|    /root    | root用户的主目录                                             |
|    /etc     | 系统配置文件以及一些程序的配置文件都在此目录                 |
|    /dev     | 外接设备会映射为此目录下的一个文件                           |
|   /media    | 把系统自动识别的U盘，光盘等挂载到此目录下                    |
|    /proc    | 一个虚拟目录，是系统内存的映射，可以获取系统以及进程的信息   |
|    /sys     | 一个虚拟目录，把硬件设备映射成文件，可以通过文件控制硬件     |
| /lost+found | 一般为空，系统异常关机时会有一些信息存入此目录               |
|    /var     | 存放一些不断变化增长的东西，例如：各种日志文件等             |
|  /usr/bin   | 用户程序目录                                                 |
|  /usr/sbin  | 需要超级用户权限运行的程序所在的目录                         |
|    /tmp     | 存放临时文件的目录                                           |
