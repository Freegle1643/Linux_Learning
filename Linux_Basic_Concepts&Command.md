## Linux基础操作

### 杂碎

#### 目录树状结构（directory tree）

Linux内所有数据、资料都是以文件的形态来呈现的（甚至包括lock这个状态信息），理解Linux的文件目录就显得至关重要。Linux的目录树架构是以根目录（`root`，即/）为主，向下呈现分支状的目录结构，所有文档都与目录树有关。

![Linux directory tree](http://markdownnotebucket-1251801748.cossh.myqcloud.com/linux%2520learning/linux_file_system_tree.jpg)

###### *图片来自 boni.ws*







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

 







