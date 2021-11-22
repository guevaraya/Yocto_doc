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

SDK 可安装到任意机器上并可用于开发应用程序，镜像和内核。SDK 甚至可被 QA 工程师或版本工程师使用。本质上说已经安装 SDK 的机器就没有必要和已安装 Yocto 项目的机器是两回事。一个开发者可以独立的编译和测试机器上的对象，然后当这个对象可以准备要集成到镜像里的时候，就可以在安装了Yocto工程的机器上很容易的构建。一旦这个对象可用了，镜像就可以直接用 Ycoto 项目重新生成新镜像。

你只需参照下面这些步骤：

1. **为你的目标硬件安装 SDK：** 关于如何安装SDK的信息，请查看[安装 SDK ][12]部分
2. **下载或编译目标镜像：** Yocto 项目支持多个目标体系架构，并有多个预编译内核镜像和文件系统镜像。

如果你将要在目标硬件上开发应用程序，要到 [machines][13] 下载目录选择一个目标机型目录来下载内核镜像和根文件系统。下载目录包含用于在实际硬件的开发支持。例如，目录里包含 `.hddimg` 文件将内核镜像的文件系统，启动加载器等的合成到一起。请确保获取的文件是你开发过程中专门需要的。

如果你正在开发应用程序然后在 QEMU 模拟器上 运行和测试，请到  [machines/qemu][14] 目录下载。从这目录进入选择你要的目标体系架构（例如：qemux86_64 对应英特尔® 64位架构的）。下载内核，根文件系统和过程所需其他的文件。

> 提示
>
> 使用 QEMU 根文件系统，你需要解压它。查看[“解压根文件系统”][15]部分获取关于如何解压根文件系统的信息.

3. **开发和测试应用程序：** 这时，你需要有工具开发你的应用程序。如果你需要独立安装和使用 QEMU 模拟器，你可以去 [QEMU 主页][16]下载和学习关于模拟器。查看 Yocto 项目开发任务手册的[“快速使用模拟器（QEMU）”][17]章节获取在 Yocto 项目使用 QEMU 的相关信息。

本手册余下部分主要描述如何使用可扩展和标准的 SDK。这些信息也在附录的表格里，主要是关于如何编译，安装和修改 SDK。


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

