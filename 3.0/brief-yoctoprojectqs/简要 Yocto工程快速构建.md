[#]: translator: (guevaraya)
[#]: reviewer: ( )
[#]: publisher: ( )
[#]: url: ( https://www.yoctoproject.org/docs/3.0/brief-yoctoprojectqs/brief-yoctoprojectqs.html)
[#]: subject: (Yocto Project Quick Build)

快速构建 Yocto 工程
======
所有权@ 2010-2019 Linux基金会

此文允许被复制，分发以及或者修改此文档，但需要遵循知识共享组织发布的共享协议[2.0 UK: England & Wales][1]条款

热烈欢迎！
======
热烈欢迎！这篇简短的文档指导你通过 Yocto 工程完成一个典型镜像的构建。文档也介绍了如何为特定硬件的配置构建。你也可以用 Yocto 构建一个 叫Poky的嵌入式参考系统。

小提示：
>+ 本文的例子假定你本地使用的 Linux 系统是最新的 Ubuntu 发行版。如果用 Yocto 工程构建一个镜像（[主机端][2]）的机器不是原生的 Linux 系统，你可以仍旧通过 Poky容器在跨平台（CROPS）基础上完成这三步。可在 Yocto 工程的开发任务手册的[“设置使用交叉平台（CROPS）”][3]段落查看更多信息。
>+ 不能使用[Windows子系统Linux（WSL）][4]构建主机。Yocto 工程不兼容 WSL。 

如果你想了解更多关于 Yocto 工程的概念或背景信息，请参照 Yocto 工程的概述和基础手册。

可兼容的 Linux 发行版
======

请确保你的[主机][5]满足一下需求：
+ 50 GB 的剩余空间
+ 可支持的 Linux 发行版（例如：最新的发行版Fedora，openSUSE，CentOS，Debian，或 Ubuntu）。支持 Yocto 工程的 Linux 发行版列表，请见：Yocto 工程的开发任务手册的[Linux发行版][6]章节。
+ Git 1.8.3.1 以上版本
+ tar 1.27 以上版本
+ Python 3.4.0 以上版本
如果构建的主机不满足以上列表任一版本要求，你可采取一定措施构建系统确保可以使用 Yocto 工程。可参看 Yocto 工程参考手册的[GIT,Tar Python版本需求]章节获取更多信息。

构建主机应用
======

You must install essential host packages on your build host. The following command installs the host packages based on an Ubuntu distribution:
你需要安装一些必要的主机应用。下面是在 Ubuntu 发行版上安装主机应用的命令：

提示
> 在所有兼容的 Linux 发行版上需要安装的主机应用，请见 Yocto 工程参考手册的[“构建主机的必要应用”][6]章节。
``` 
$ sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \
     build-essential chrpath socat cpio python python3 python3-pip python3-pexpect \
     xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev \
     pylint3 xterm
``` 
使用 Git 克隆 Poky
======

一旦你的机器按照安装说明已经完成，你需要在你的构建主机上获取一个 Poky 仓库的拷贝。用以下命令拷贝 Poky 仓。
``` 
     $ git clone git://git.yoctoproject.org/poky
     Cloning into 'poky'...
     remote: Counting objects: 432160, done.
     remote: Compressing objects: 100% (102056/102056), done.
     remote: Total 432160 (delta 323116), reused 432037 (delta 323000)
     Receiving objects: 100% (432160/432160), 153.81 MiB | 8.54 MiB/s, done.
     Resolving deltas: 100% (323116/323116), done.
     Checking connectivity... done.
```          
转到 poky 目录下并查看所有标签：
```
     $ cd poky
     $ git fetch --tags
     $ git tag
     1.1_M1.final
     1.1_M1.rc1
     1.1_M1.rc2
     1.1_M2.final
     1.1_M2.rc1
        .
        .
        .
     yocto-2.5
     yocto-2.5.1
     yocto-2.5.2
     yocto-2.6
     yocto-2.6.1
     yocto-2.6.2
     yocto-2.7
     yocto_1.5_M5.rc8
 ```           
 
例如，检出 yocto-3.0 的分支：

```
     $ git checkout tags/yocto-3.0 -b my-yocto-3.0
     Switched to a new branch 'my-yocto-3.0'
```

上面的 Git 检出命令创建了一个名为 my-yocto-3.0 的本地分支。这个分支的文件与 yocto 工程 yocto-3.0 的 “zeus” 开发分支的文件完全一样。

获取更多 Yocto 工程有关仓库的选项和信息，请见 Yocto 工程开发任务手册的[“本地 Yocto 工程源码文件”][7]章节。

via: https://www.yoctoproject.org/docs/3.0/brief-yoctoprojectqs/brief-yoctoprojectqs.html

[1]: https://creativecommons.org/licenses/by-sa/2.0/uk/
[2]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#hardware-build-system-term
[3]: http://www.yoctoproject.org/docs/3.0/dev-manual/dev-manual.html#setting-up-to-use-crops
[4]: https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux
[5]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#hardware-build-system-term
[6]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#required-packages-for-the-build-host
[7]: http://www.yoctoproject.org/docs/3.0/dev-manual/dev-manual.html#locating-yocto-project-source-files
