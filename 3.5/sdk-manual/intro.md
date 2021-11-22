[#]: subject: "快速构建 Yocto 项目"
[#]: via: "https://docs.yoctoproject.org/brief-yoctoprojectqs/index.html"
[#]: author: "The Linux Foundation"
[#]: collector: "guevaraya"
[#]: translator: "guevaraya "
[#]: reviewer: " "
[#]: publisher: " "
[#]: url: ""


1 介绍
====

1.1 eSDK 介绍
=================
欢迎阅读 Yocto 项目应用开发和可扩展软件套件的 eSDK 手册。本手册主要涉及如何使用 Yocto 可扩展套件和标准SDK 来开发应用程序和构建镜像的相关信息。

整个 SDK 的组成如下：
* **交叉工具链**：工具链包含一个编译器，调试器和其他相关工具。
* **库文件及其头文件，符号表**：指定镜像（例如 SDK编译的镜像匹配等）的库文件，头文件和符号表
* **环境变量配置脚本**：这个 *.sh* 脚本文件只需要运行一次，通过定义变量和做一些使用 SDK 的准备工作来配置交叉开发的环境变量。

另外，一个可扩展 SDK 里的工具允许你容易的添加一个应用程序和镜像的库文件，修改现有组件的源码，测试目标硬件的变化，并很容易的集成一个应用层序到 [OpenEmbedded 构建系统][4]里。

你可以用其中一个 SDK 去独立的开发和测试在一些目标机器运行的代码。这些 SDK 是完全独立的。二进制程序是链接本地 libc，这样使其对目标系统没有依赖。为了实现这点，动态加载器的指针在安装的时候配置后地址就不能动态改变了。这就是为什么有 populate_sdk 和 populate_sdk_ext 封装的分别。

SDK 的另一个特性是用一套交叉工具链是构建任意指定的指令架构。这个特性的好处是目标硬件可以统一通过编译选项控制 gcc 编译。这些选项通过环境变量脚本配置生效并其包含在变量里，例如 [CC][5] 和 [LD][6]。这样会减少工具使用的内存占用。要知道，尽管是这样，每个目标设备仍然需要它的 sysroot 因为这些二进制是针对特定目标板的。

SDK 开发环境组成如下：

* 独立的 SDK 是指定架构的交叉编译和对应的 sysroot（目标文件和本地文件），这些都是通过 OpenEmbedded 构建系统编译处理的（例如 SDK）。交叉工具链和 sysroot 都是基于 Metadata 的配置和扩展的，这个允许在宿主机器上交叉开发目标硬件。另外，可扩展 SDK 包含了 devtool 的功能。
* QEMU 模拟器可以允许模拟目标硬件。它不是 SDK 里的一部分。你必须单独编译和引用这个模拟器。QENU 在围绕 SDK 程序开发扮演重要的角色。

总得来说，可扩展和标准的 SDK 有很多共同的特性。但是可扩展 SDK 作为强大的开发工具帮助你更快速的开发应用程序。下面的表格汇总了标准 SDK 和可扩展 SDK 之间在构建上的一些区别：


|特性<img width=100/> |标准SDK<img width=200/> |可扩展SDK <img width=300/> |
|-|-|-|
|交叉链|是|是<sup>①</sup>|
|调试器|是|是<sup>①</sup>|
|大小|大约100Mb|1Gb以上（迷你版本工具链大约300Mb）|
|devtol|否|是|
|编译镜像|否|是|
|可更新|否|是|
|可管理的 sysroot<sup>②</sup>|否|是|
|包安装|否<sup>③</sup>|是<sup>④</sup>|
|结构方式|安装包|共享状态|

<sub>
① 如果[SDK_EXT_TYPE][7] 等于 “full” 或者 [SDK_INCLUDE_TOOLCHAIN][8] 等于 “1”的时候（默认值），可扩展 SDK 包含交叉链和调试器。<br>
② Sysroot 是通过 devltool 来管理的。因此如果你尝试添加额外的库，会有小概率损坏 SDK 的sysroot。<br>
③ 你可以添加运行时包管理到标准 SDK，这个默认是不支持的。<br>
④ 你必须为想要安装 “包” 的用户构建和编译共享模式变量给可扩展 SDK 的用户。<br>
</sub>


1.1.1. 交叉编译工具链
======
[交叉开发工具链][9]包含交叉编译器，交叉链接器和交叉调试器，这些用来开发目标硬件的用户应用程序。另外，对于可扩展 SDK，工具链也内置了`devtool` 功能。这样工具链可通过运行 SDK 安装脚本来创建，或通过[构建目录][10]的 metadata 配置或目标设备的扩展组件来创建。交叉工具链与对应的目标设备 sysroot 才能工作。

1.1.2. Sysroots
======
宿主和目标板的 sysroot 包含所依赖的头文件和库文件，它们最终生成的二进制文件将运行在在目标架构上。目标板的 sysroot 通过 OpenEmbedded 构建系统编译到目标版本的根文件系统镜像里，使用的 metadata 的配置和构建交叉工具链的配置一样。

1.1.3. QEMU 模拟器
======
QEMU 模拟器可以模拟应用程序或镜像在你的目标硬件设备上运行。QEMU不属于SDK但如果你做了以下事情会自动被安装：

* 通过克隆 poky 的Git仓创建了 [Source 目录][11]并 source 了环境变量的配置脚本
* 下载了 Yocto 项目发行包并解压到 Source 目录同时 source 了环境变量的配置脚本
* 安装了交叉工具链的tar包并 source 了环境变量的配置脚本
* 
1.2 SDK 开发模型
======
从本质上说，SDK 的开发过程概括如下：

![SDK环境变量][18]

The SDK is installed on any machine and can be used to develop applications,
images, and kernels. An SDK can even be used by a QA Engineer or Release
Engineer. The fundamental concept is that the machine that has the SDK
installed does not have to be associated with the machine that has the
Yocto Project installed. A developer can independently compile and test
an object on their machine and then, when the object is ready for
integration into an image, they can simply make it available to the
machine that has the Yocto Project. Once the object is available, the
image can be rebuilt using the Yocto Project to produce the modified
image.

You just need to follow these general steps:

1. *Install the SDK for your target hardware:* For information on how to
   install the SDK, see the ":ref:`sdk-manual/using:installing the sdk`"
   section.

2. *Download or Build the Target Image:* The Yocto Project supports
   several target architectures and has many pre-built kernel images and
   root filesystem images.

   If you are going to develop your application on hardware, go to the
   :yocto_dl:`machines </releases/yocto/yocto-&DISTRO;/machines/>` download area and choose a
   target machine area from which to download the kernel image and root
   filesystem. This download area could have several files in it that
   support development using actual hardware. For example, the area
   might contain ``.hddimg`` files that combine the kernel image with
   the filesystem, boot loaders, and so forth. Be sure to get the files
   you need for your particular development process.

   If you are going to develop your application and then run and test it
   using the QEMU emulator, go to the
   :yocto_dl:`machines/qemu </releases/yocto/yocto-&DISTRO;/machines/qemu>` download area. From this
   area, go down into the directory for your target architecture (e.g.
   ``qemux86_64`` for an Intel-based 64-bit architecture). Download the
   kernel, root filesystem, and any other files you need for your
   process.

   .. note::

      To use the root filesystem in QEMU, you need to extract it. See the
      ":ref:`sdk-manual/appendix-obtain:extracting the root filesystem`"
      section for information on how to do this extraction.

3. *Develop and Test your Application:* At this point, you have the
   tools to develop your application. If you need to separately install
   and use the QEMU emulator, you can go to `QEMU Home
   Page <https://wiki.qemu.org/Main_Page>`__ to download and learn about
   the emulator. See the ":doc:`/dev-manual/qemu`" chapter in the
   Yocto Project Development Tasks Manual for information on using QEMU
   within the Yocto Project.

The remainder of this manual describes how to use the extensible and
standard SDKs. There is also information in appendix form describing
how you can build, install, and modify an SDK.



[1]: https://creativecommons.org/licenses/by-sa/2.0/uk/
[2]: http://www.yoctoproject.org/documentation
[3]: https://wiki.yoctoproject.org/wiki/Releases
[4]: https://docs.yoctoproject.org/ref-manual/terms.html#term-OpenEmbedded-Build-System
[5]: https://docs.yoctoproject.org/ref-manual/variables.html#term-CC
[6]: https://docs.yoctoproject.org/ref-manual/variables.html#term-LD
[7]: https://docs.yoctoproject.org/ref-manual/variables.html#term-SDK_EXT_TYPE
[8]: https://docs.yoctoproject.org/ref-manual/variables.html#term-SDK_INCLUDE_TOOLCHAIN
[9]: https://docs.yoctoproject.org/ref-manual/terms.html#term-Cross-Development-Toolchain
[10]: https://docs.yoctoproject.org/ref-manual/terms.html#term-Build-Directory
[11]: https://docs.yoctoproject.org/ref-manual/terms.html#term-Source-Directory
[12]: https://docs.yoctoproject.org/sdk-manual/using.html#installing-the-sdk
[13]: https://downloads.yoctoproject.org/releases/yocto/yocto-3.4/machines/
[14]: https://downloads.yoctoproject.org/releases/yocto/yocto-3.4/machines/qemu
[15]: https://docs.yoctoproject.org/sdk-manual/appendix-obtain.html#extracting-the-root-filesystem
[16]: https://wiki.qemu.org/Main_Page
[17]: https://docs.yoctoproject.org/dev-manual/qemu.html
[18]: https://docs.yoctoproject.org/_images/sdk-environment.png


---
via: https://docs.yoctoproject.org/sdk-manual/intro.html

译者：[guevara](https://github.com/guevaraya)
校对：[校对者ID](https://github.com/校对者ID)
版权所有© 2010-2019 Linux 基金会

