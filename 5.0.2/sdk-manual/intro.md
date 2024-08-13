

<a id="sdk-intro">第一章 入门介绍</a>
===

  **目录索引**
  * [1.1. 入门介绍](#sdk-manual-intro)
    * [1.1.1. 交叉编译工具链](#the-cross-development-toolchain)
    * [1.1.2. Sysroots](#sysroot)
    * [1.1.3. QEMU 模拟器](#the-qemu-emulator)
  * [1.2. SDK 开发模型](#sdk-development-model)
  
<a id="sdk-manual-intro">1.1. 入门介绍</a>
====
欢迎阅读 Yocto 项目应用程序开发和可扩展软件开发的 SDK手册。本手册主要涉及如何使用 Yocto 可扩展和标准 SDK 来开发应用程序和镜像的相关信息。
> **小提示**
>
> Yocto 项目 2.0 早前版本，应用程序开发主要是通过使用 ADT 套件，常见尽可能的交叉编译工具链和其他工具。yocto 项目 2.1 版本以后应用程序开发用丰富的可扩展 SDK 和 更多传统的标准 SDK 可以完成。
所有的 SDK 的组成如下：
* **交叉工具链**：工具链包含一个编译器，调试器和许多各种各样的工具。
* **库文件及其头文件，符号表**：指定镜像（例如他们匹配的镜像）的库文件，头文件和符号表
* **环境变量配置脚本**：这个 *.sh* 脚本文件只需要运行一次，通过定义变量和做一些使用 SDK 的准备工作来配置交叉开发的环境变量。

另外，一个可扩展 SDK 的工具允许你容易的添加一个应用程序和镜像的库文件，修改现有组件的源码，测试目标硬件的变化，并很容易的集成一个应用层序到 [OpenEmbedded 构建系统][4]里。

你可以用一个 SDK 去独立的开发和测试代码使其运行在一些在目标机器上。这些 SDK 是完全独立的。二进制程序是链接到他们本地 libc 的拷贝，这样使其对目标系统没有依赖。为了实现这点，动态加载器的指针在安装的时候配置后地址就不能动态改变了。这就是为什么有封装 populate_sdk 和 populate_sdk_ext 的分别。

SDK 的另一个特性是构造一整套的交叉工具链是为了支持任意给定的指令架构。这个特性的好处是目标硬件可以通过编译选项控制 gcc 编译。这些选项通过环境变量脚本设置并包含在变量里，例如 [CC][5] 和 [LD][6]。这样会减少工具使用的内存占用。要知道，尽管这样，每个目标仍然需要一个 sysroot 应该这些二进制是针对特定目标板的。

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
① 如果SDK_EXT_TYPE 等于 “full” 或者 SDK_INCLUDE_TOOLCHAIN 等于 “1”的时候（默认值），可扩展 SDK 包含交叉链和调试器。<br>
② Sysroot 是通过 devltool 来管理的。因此如果你尝试添加额外的库，会有小概率损坏 SDK 的sysroot。<br>
③ 你可以添加运行时包管理到标准 SDK，这个默认是不支持的。<br>
④ 你必须为想要安装 “包” 的用户构建和编译共享模式变量给可扩展 SDK 的用户。<br>
</sub>

</sub>

<a id="the-cross-development-toolchain">1.1.1. 交叉开发工具链 </a>
====

[交叉开发工具链][7]包含交叉编译器，交叉链接器和交叉调试器，这些用来开发目标硬件的用户应用程序。
另外，对于可扩展 SDK，工具链也内置`devtool` 功能。这样工具链可通过运行 SDK 安装脚本来创建，或通过[构建目录][8]的 metadata 配置或目标设备的扩展来实现。交叉工具链需要对应的目标设备 sysroot 才能工作 。

 <a id="sysroot">1.1.2. Sysroots</a>
=====
宿主和目标板的 sysroot 包含所依赖的头文件和库文件，最终生成二进制文件在目标架构上运行。目标板的 sysroot 通过 OpenEmbedded 构建系统编译到目标版本的根文件系统镜像里，使用的 metadata 的配置和构建交叉工具链的一样。


<a id="#the-qemu-emulator">1.1.3. QEMU 模拟器</a>
=====
QEMU 模拟器可以模拟在应用程序或镜像运行在你的硬件上。QEMU 不是 SDK 的一部分，但是它有几种生成方法：
* 如果你克隆了 poky 的 GIT 仓来创建的[源码目录][9]并且你已经执行了环境变量的配置脚本，QEMU 就已经安装好并自动可用的。
* 如果你下载的是 Yocto 项目发行版并解压创建的源码目录，你也执行了环境变量配置脚本，QEMU 就已经安装好并自动可用的。
* 如果你已经安装了 交叉工具链的压缩包并你已经运行了工具链的环境变量的配置脚本，QEMU 也是已经安装并自动可用的。

<a id="#sdk-development-model">1.2. SDK 开发模型</a>
=====
从本质上看，SDK 在开发流程中如下所示：

 ![1.2. SDK 开发模型][10]

SDK 可安装到任意机器上并可用于开发应用程序，镜像和内核。SDK 甚至可被 QA 工程师或版本工程师使用。本质上说已经安装 SDK 的机器就没有必要和已安装 Yocto 项目的机器是两回事。一个开发者可以独立的编译和测试机器上的对象，然后当这个对象可以准备要集成到镜像里的时候，就可以在安装了Yocto工程的机器上很容易的构建。一旦这个对象可用了，镜像就可以直接用 Ycoto 项目重新生成新镜像。

建议按照下面这些步骤：

1. **为你的目标硬件安装 SDK：** 关于如何安装SDK的信息，请查看[安装 SDK ][11]部分

2. **下载或编译目标镜像：** Yocto 项目支持多个目标体系架构，并有多个预编译内核镜像和文件系统镜像。

如果你将要在目标硬件上开发应用程序，要到 [machines][12] 下载目录选择一个目标机型目录来下载内核镜像和根文件系统。下载目录包含用于在实际硬件的开发支持。例如，目录里包含 `.hddimg` 文件将内核镜像的文件系统，启动加载器等的合成到一起。请确保获取的文件是你开发过程中专门需要的。

如果你正在开发应用程序然后在 QEMU 模拟器上 运行和测试，请到  [machines/qemu][12] 目录下载。从这目录进入选择你要的目标体系架构（例如：qemux86_64 对应英特尔® 64位架构的）。下载内核，根文件系统和过程所需其他的文件。

> 提示
>
> 使用 QEMU 根文件系统，你需要解压它。查看[“解压根文件系统”][14]部分获取关于如何解压根文件系统的信息.

3. **开发和测试应用程序：** 这时，你需要有工具开发你的应用程序。如果你需要独立安装和使用 QEMU 模拟器，你可以去 [QEMU 主页][13]下载和学习关于模拟器。查看 Yocto 项目开发任务手册的[“快速使用模拟器（QEMU）”][14]章节获取在 Yocto 项目使用 QEMU 的相关信息。

本手册余下部分主要描述如何使用可扩展和标准的 SDK。这些信息也在附录的表格里，主要是关于如何编译，安装和修改 SDK。

原文: https://docs.yoctoproject.org/5.0.2/sdk-manual/intro.html

译者：[巴龙](https://github.com/guevaraya)
校对：[校对者ID](https://github.com/校对者ID)
版权所有© 2010-2019 Linux 基金会

[1]: https://creativecommons.org/licenses/by-sa/2.0/uk/
[2]: http://www.yoctoproject.org/documentation
[3]: https://wiki.yoctoproject.org/wiki/Releases
[4]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#build-system-term
[5]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#var-CC
[6]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#var-LD 
[7]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#cross-development-toolchain
[8]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#build-directory
[9]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#source-directory
[10]: https://www.yoctoproject.org/docs/3.0/sdk-manual/figures/sdk-environment.png
[11]: https://www.yoctoproject.org/docs/3.0/sdk-manual/sdk-manual.html#sdk-installing-the-sdk
[12]: http://downloads.yoctoproject.org/releases/yocto/yocto-3.0/machines
[13]: http://downloads.yoctoproject.org/releases/yocto/yocto-3.0/machines/qemu
[14]: https://www.yoctoproject.org/docs/3.0/sdk-manual/sdk-manual.html#sdk-extracting-the-root-filesystem
[15]: http://wiki.qemu.org/Main_Page
[16]: http://www.yoctoproject.org/docs/3.0/dev-manual/dev-manual.html#dev-manual-qemu
[17]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#var-CC
[18]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#var-LD
[19]: https://www.yoctoproject.org/docs/3.0/overview-manual/overview-manual.html#git
[20]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#ref-devtool-reference
[21]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#devtool-the-workspace-layer-structure
[22]: https://www.yoctoproject.org/docs/3.0/sdk-manual/figures/sdk-devtool-add-flow.png
[23]: https://www.yoctoproject.org/docs/3.0/sdk-manual/figures/sdk-devtool-modify-flow.png
[24]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#var-SRC_URI