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
> 在所有兼容的 Linux 发行版上需要安装的主机应用，请见 Yocto 工程参考手册的“构建主机的必要应用”章节。
``` 
$ sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \
     build-essential chrpath socat cpio python python3 python3-pip python3-pexpect \
     xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev \
     pylint3 xterm
``` 


via: https://www.yoctoproject.org/docs/3.0/brief-yoctoprojectqs/brief-yoctoprojectqs.html

[1]: https://creativecommons.org/licenses/by-sa/2.0/uk/
[2]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#hardware-build-system-term
[3]: http://www.yoctoproject.org/docs/3.0/dev-manual/dev-manual.html#setting-up-to-use-crops
[4]: https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux
[5]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#hardware-build-system-term
