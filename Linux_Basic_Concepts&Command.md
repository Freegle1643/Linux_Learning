## Linux基础操作

### 杂碎

#### 目录树状结构（directory tree）

Linux内所有数据、资料都是以文件的形态来呈现的（甚至包括lock这个状态信息），理解Linux的文件目录就显得至关重要。Linux的目录树架构是以根目录（`root`，即/）为主，向下呈现分支状的目录结构，所有文档都与目录树有关。

![Linux directory tree](http://markdownnotebucket-1251801748.cossh.myqcloud.com/linux%2520learning/linux_file_system_tree.jpg)

###### *图片来自 boni.ws*



#### 文件系统的挂载

所谓挂载，就是利用一个目录当作接入点（进入点，挂载点），也就是说，进入了某个挂载点就可以读取该挂载点所联系的向下的分割槽中的所有文件目录，这个进入目录就叫挂载点。Linux系统最重要的就是根目录，所以该目录一定是要挂在到一个分割槽的，至于其他的目录我们可以根据需求来选择性挂载，或者挂载到不同的物理区域等。

![Linux文件系统挂载](http://markdownnotebucket-1251801748.cossh.myqcloud.com/linux%2520learning/Linux_file_sys_mount.png)

###### *图片来自鸟哥的Linux私房菜 vbird.org*



#### TTY是什么意思？

在早期的时候用户终端连接电脑是通过一种叫electromechanical teleprinters 或者 teletypewriters (TeleTYpewriter, TTY)的东西，从那之后这样的叫法就被延续下来了。

#### PTS是什么意思？

意思是pseudo terminal slave. 

##### PTS v.s. TTY

The difference between TTY and PTS is the type of connection to the computer. TTY ports are direct connections to the computer such as a keyboard/mouse or a serial connection to the device. PTS connections are SSH connections or telnet connections. All of these connections can connect to a shell which will allow you to issue commands to the computer.

*https://www.question-defense.com/2009/09/11/what-do-pts-and-tty-mean-on-linux-what-is-the-difference-between-the-two-terminal-types*

简而言之就是你用命令行什么的来操作电脑，你都会被视为一个PTS进程，例如：

```bash
  PID TTY          TIME CMD
23480 pts/1    00:00:00 ps
```

如果我们想通过TTY来使用系统，则需要在Linux下使用**[Ctrl] + [Alt] + [F1]~[F6]** 来进行切换，F1-F6对应了不同的的tty终端，这个时候如果你在查看自己所在进程：

```bash
 PID TTY          TIME CMD
4294 tty3     00:00:00 ps
```

*唯一不太舒服的就是如果是在虚拟机或者X11远程图形界面登陆的时候这样的切换会导致你的机器恢复到默认的分辨率，有一些烦人。*



#### 安装更新时遇到的无法lock的报错

有时我们在运行 `sudo apt-get update`  |  `sudo apt-get upgrade`的时候可能会出现

```bash
E: Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable) 
E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?

E: Could not get lock /var/cache/apt/archives/lock - open (11: Resource temporarily unavailable)
E: Unable to lock directory /var/cache/apt/archives/
```

这样无法lock住一个dpkg | archieves 的错，这可能是由于之前有过用户在进行update操作的时候没有正常退出，导致这个文件还在被上锁阶段，在确定没有用户真的在进行update或者相关更新、安装操作后，我们可以使用`sudo rm /var/lib/dpkg/lock`  |  ` sudo rm /var/cache/apt/archives/lock`来解除上锁状态。



#### 验证开机时启动的[boot image](https://en.wikipedia.org/wiki/Boot_image)和[initial ramdisk](https://en.wikipedia.org/wiki/Initial_ramdisk)

- 什么是initial ramdisk ？

  In computing, **initrd** (*initial ramdisk*) is a scheme for loading a temporary root [file system](https://www.wikiwand.com/en/File_system) into [memory](https://www.wikiwand.com/en/Computer_memory), which may be used as part of the [Linux startup process](https://www.wikiwand.com/en/Linux_startup_process). `initrd` and `initramfs` refer to two different methods of achieving this. Both are commonly used to make preparations before the real [root](https://www.wikiwand.com/en/Root_directory) file system can be [mounted](https://www.wikiwand.com/en/Mount_(Unix)).

```bash
cat /proc/cmdline
```

上述命令可以列出你启动时所加载的boot image和initial ramdisk，多用于在自定义Linux Kernel后确认自己新的内核是否得以加载。

cmdline中包含了Arguments passed to the Linux kernel at boot time. 









#### Linux的启动器（Boot Loader）

常见的又LILO和GRUB（现在的发行版本多用此）

- [LILO](https://en.wikipedia.org/wiki/LILO_(boot_loader))

  **LILO** (**LI**nux **LO**ader) is a [boot loader](https://www.wikiwand.com/en/Boot_loader)for [Linux](https://www.wikiwand.com/en/Linux) and was the default boot loader for most [Linux distributions](https://www.wikiwand.com/en/Linux_distribution)in the years after the popularity of [loadlin](https://www.wikiwand.com/en/Loadlin).

- [GNU GRUB（GRUB）](https://en.wikipedia.org/wiki/GNU_GRUB)

  GNU GRUB was developed from a package called the *Grand Unified Bootloader* (a play on [Grand Unified Theory](https://www.wikiwand.com/en/Grand_Unified_Theory)(https://www.wikiwand.com/en/GNU_GRUB#citenote5)).




### 常用命令

#### 查看以某些规律开头的所有指令

```bash
ubuntu@VM-250-138-ubuntu:~$ g[tab][tab]
```

输入某个你想得起来的开头，点击两次Tab按钮，就可以看到所有可用的以g开头的命令了。



#### 查看某个指令的使用手册

```bash
ubuntu@VM-250-138-ubuntu:~$ man sudo
```

man是manual的简写，输入man [command]可以查看相关的使用手册



#### 更新和升级软件

`apt-get update`

updates the list of available packages and their versions, but it does not install or upgrade any packages.

`apt-get upgrade`

actually installs newer versions of the packages you have. After updating the lists, the package manager knows about available updates for the software you have installed. This is why you first want to `update`

需要先跑`update`再跑`upgrade`，虽然这并不是必须的，但是我们总是觉得先更新软件源再去真正的更新和下载软件是佳策。



#### 文件操作

##### 删除

`rm` 删除一个文件

`rmdir` 删除空目录

`rm -rf` 删除非空目录

##### 复制

`cp` 具体可以使用`cp --help`查看

常用：

`cp /path/to/somefile /path/to/destination ` 复制文件

`cp -R /path/to/a/folder /path/to/destination` 复制文件夹（包含文件夹内容） 







