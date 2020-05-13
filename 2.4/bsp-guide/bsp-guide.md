[#]: translator: (guevaraya)
[#]: reviewer: ( )
[#]: publisher: ( )
[#]: url: ( https://www.yoctoproject.org/docs/)
[#]: subject: (Yocto 项目 BSP 开发指导)

Yocto 项目 BSP 开发指导
======

<p align="right">Scott Rifenbark<br>
Scott的文档服务公司<br>
<a href="mailto:srifenbark@gmail.com"><font size=100>&lt;srifenbark@gmail.com&gt;</font></a>
</p>

版权所有@ 2010-2017  Linux 基金会

此文允许被复制，分发以及或者修改此文档，但需要遵循知识共享组织发布的共享协议2.0 UK: England & Wales条款

**手册说明**

* 这个 Yocto 项目 BSP 的开发指导版本是针对 Yocto 项目正式版本 2.4 的。请确认你拿到的是正式版本对应的最新手册，并使用 [Yocto 项目文档页面][2]的手册。
* 若需此文档相关的 Yocto 项目其他的版本，到 [Yocto 项目文档页面][2]点击下拉框里的 “Active Releases” 按钮，来选择你想要的相关 Yocto 项目文档。
* 报告这个文档的任何错误或问题，请发邮件到 Yocto 项目讨论组 yocto@yoctoproject.com 或 登陆 freenode 的 #yocto 频道。

|**修订历史**||
|-|-|
|版本 0.9	24 2010年11月|2010年11月24日|
|发布 Yocto 项目初始稿 0.9 版本||
|版本 1.0	|2011年4月6日|
|发布 Yocto 项目 1.0 版本|
|版本 1.0.1|2011年6月23日|
|发布 Yocto 项目 1.0.1 版本|
|版本 1.1|2011年10月6日|
|发布 Yocto 项目 1.1 版本|
|版本 1.2|2012年4月|
|发布 Yocto 项目 1.2 版本|
|版本 1.3	|2012年10月|
|发布 Yocto 项目 1.3 版本|
|版本 1.4|2013年4月|
|发布 Yocto 项目 1.4 版本|
|版本 1.5|2013年10月|
|发布 Yocto 项目 1.5 版本|
|版本 1.5.1 |2014年1月|
|发布 Yocto 项目 1.5.1 版本|
|版本 1.6|2014年4月|
|发布 Yocto 项目 1.6 版本|
|版本 1.7	|2014年10月|
|发布 Yocto 项目 1.7 版本|
|版本 1.8|2015年4月|
|发布 Yocto 项目 1.8 版本|
|版本2.0|2015年10月|
|发布 Yocto 项目 2.0 版本|
|版本 2.1|2016年4月|
|发布 Yocto 项目 2.1 版本|
|版本 2.2|2016年10月|
|发布 Yocto 项目 2.2 版本|
|版本2.3|2017年6月|
|发布 Yocto 项目 2.3 版本|
|版本 2.4|2017年10月|
|发布 Yocto 项目 2.4 版本|

**目录**
<!-- GFM-TOC -->
* [1. 板级开发包（BSP） - 开发指导](#bsp)
  * [1.1. BSP 层](#bsp-layers)
  * [1.2. 准备您用于开发 BSP 层的编译主机](#preparing-your-build-host-to-work-with-bsp-layers)
  * [1.3. 文件系统布局示例](#bsp-filelayout)
    * [1.3.1. 授权许可文件](#bsp-filelayout-license)
    * [1.3.2. 自述说明文件](#bsp-filelayout-readme)
    * [1.3.3. 源码自述说明文件](#bsp-filelayout-readme-sources)
    * [1.3.4. 用户二进制预编译](#bsp-filelayout-binary)
    * [1.3.5. 层配置文件](#bsp-filelayout-layer)
    * [1.3.6. 硬件配置选项](#bsp-filelayout-machine)
    * [1.3.7. 其他特定的BSP配方文件](#bsp-filelayout-misc-recipes)
    * [1.3.8. 支持图形显示的相关文件](#bsp-filelayout-recipes-graphics)
    * [1.3.9. Linux 内核配置文件](#bsp-filelayout-kernel)
  * [1.4. 开发一个板级支持包（BSP）](#developing-a-board-support-package-bsp)
  * [1.5. 发布 BSP 包的要求和建议](#requirements-and-recommendations-for-released-bsps)
    * [1.5.1. 发布 BSP 包的要求](#released-bsp-requirements)
    * [1.5.2. 发布 BSP 包的建议](#released-bsp-recommendations)
  * [1.6. 为 BSP 定制一个配方](#customizing-a-recipe-for-a-bsp)
  * [1.7. BSP 许可的注意事项](#bsp-licensing-considerations)
  * [1.8. 使用 Yocto 项目的 BSP 工具](#using-the-yocto-projects-bsp-tools)
    * [1.8.1. 常见特性](#common-features)
    * [1.8.2. 用 yocto-bsp 脚本创建一个新的 BSP 层](#creating-a-new-bsp-layer-using-the-yocto-bsp-script)
    * [1.8.3. 管理 yocto-kernel 的内核补丁和配置项](#managing-kernel-patches-and-config-items-with-yocto-kernel)
<!-- GFM-TOC -->


<a id="bsp">Chapter 1. Board Support Packages (BSP) - Developer's Guide</a>
======

**目录**

<!-- GFM-TOC -->
* [1.1. BSP 层](#bsp-layers)
* [1.2. 准备您用于开发 BSP 层的编译主机](#preparing-your-build-host-to-work-with-bsp-layers)
* [1.3. 文件系统布局示例](#bsp-filelayout)
  * [1.3.1. 授权许可文件](#bsp-filelayout-license)
  * [1.3.2. 自述说明文件](#bsp-filelayout-readme)
  * [1.3.3. 源码自述说明文件](#bsp-filelayout-readme-sources)
  * [1.3.4. 用户二进制预编译](#bsp-filelayout-binary)
  * [1.3.5. 层配置文件](#bsp-filelayout-layer)
  * [1.3.6. 硬件配置选项](#bsp-filelayout-machine)
  * [1.3.7. 其他特定的BSP配方文件](#bsp-filelayout-misc-recipes)
  * [1.3.8. 支持图形显示的相关文件](#bsp-filelayout-recipes-graphics)
  * [1.3.9. Linux 内核配置文件](#bsp-filelayout-kernel)
  * [1.4. 开发一个板级支持包（BSP）](#developing-a-board-support-package-bsp)
* [1.5. 发布 BSP 包的要求和建议](#requirements-and-recommendations-for-released-bsps)
  * [1.5.1. 发布 BSP 包的要求](#released-bsp-requirements)
  * [1.5.2. 发布 BSP 包的建议](#released-bsp-recommendations)
* [1.6. 为 BSP 定制一个配方](#customizing-a-recipe-for-a-bsp)
* [1.7. BSP 许可的注意事项](#bsp-licensing-considerations)
* [1.8. 使用 Yocto 项目的 BSP 工具](#using-the-yocto-projects-bsp-tools)
  * [1.8.1. 常见特性](#common-features)
  * [1.8.2. 用 yocto-bsp 脚本创建一个新的 BSP 层](#creating-a-new-bsp-layer-using-the-yocto-bsp-script)
  * [1.8.3. 管理 yocto-kernel 的内核补丁和配置项](#managing-kernel-patches-and-config-items-with-yocto-kernel)

一个板级开发包（BSP）是一个信息集合，它定义了如何支持特定硬件设备，设备集或一个硬件平台。BSP 包含设备里的硬件功能信息和内核额外需要的硬件驱动的配置信息。BSP 也罗列了除过基本且可选的平台特性的 Linux 软件栈外，还有额外需要的软件组件列表。

这个指导包含了 BSP 层信息，定义了一个组件的结构以便 BSP 包遵循一个通用的可理解的布局，讨论了如何为一个 BSP 定制一个配方，解释了 BSP 许可问题，提供了说明如何用 Yocto 项目的 [BSP 工具](#using-the-yocto-projects-bsp-tools)创建和管理一个 [BSP 层](#bsp-layers)的相关信息。

<a id="bsp-layers">1.1. BSP Layers</a>
=======

一个 BSP 层在主目录下以文件形式组成。总体上说，你可以将主目录的文件形式和内容看作是 BSP 层。尽管这不是强制要求，但 Yocto 项目中的层用以下公认的命名约定：

```
meta-bsp_name
```

字符串 “meta-” 前缀到机器名或平台名上，上面的形式是 *bsp-name*。 

> 小贴士
>
> 因为 BSP 层的命名约定是被广泛认可的，创建层的时候最好遵循它。从技术上讲，一个 BSP 层命名可以不需要以 *meta-* 开头，但是这样命名会让你可能遇到脚本混乱的状况。

鉴于 Yocto 项目的每个正式版本都支持和提供了 BSP 包，为了有助于理解 BSP 层的概念。你可以通过网站 http://git.yoctoproject.org/cgit/cgit.cgi 看到[ Yocto 项目源码库](#yocto-project-repositories)的所有层。如果你去这个网站，你会在 “Yocto Meta data Layer” 下面的列表最后看到Yocto 项目支持的 一些 BSP 层（例如：meta-raspberrypi 和 meta-intel）。这些层自身都是一个独立仓，点击这个层可以看到更多信息，其中包含有两个链接可供选择克隆到本地系统。这是一个克隆树莓派层的例子：

```
$ git clone git://git.yoctoproject.org/meta-raspberrypi
```

除了在Yocto 项目引用仓列表最下面的 BSP 层之外，*meta-yocto-bsp* 层也是 poky 仓库打包的一部分。*meta-yocto-bsp* 层维护着几个 BSP 包
，如 Beaglebone、EdgeRouter和通用版本，包含32 位和 64 位 IA 机器。

想获取 BSP 开发流程的信息，请查看[“开发一个板级开发包（BSP）”](#developing-a-board-support-package-bsp)章节。想获取更多如何从 Git 仓建立一个本地源码拷贝的信息，请查看 Yocto 项目开发任务手册的 [“使用 Yocto 项目源码文件”](#working-with-yocto-project-source-files) 章节。

层的基础目录（*meta-bsp_name*）便是 BSP 层的根目录。这个根目录通过变量 [BBLAYERS](#var-BBLAYERS) 来添加到对应的配置文件，即在[构建目录](#build-directory)下的 *conf/bblayers.conf* 。它在你运行 OpenEmbedded 的编译环境配置脚本（如：[oe-init-build-env](#structure-core-script)）之后才会生效。添加此根目录让 OpenEmbedded 构建系统可识别 BSP 的定义并根据它编译镜像。这是一个例子：

```
     BBLAYERS ?= " \
       /usr/local/src/yocto/meta \
       /usr/local/src/yocto/meta-poky \
       /usr/local/src/yocto/meta-yocto-bsp \
       /usr/local/src/yocto/meta-mylayer \
       "
```

一些额外的 BSP 层需要放到 BSP 的根目录下为实现特定的功能。这种情况下，为构建 BSP你也需要添加这些层到 [BBLAYERS](#var-BBLAYERS) 变量里。你也必须要按照 BSP 的 *README* 文件的“依赖关系”段落里对额外层的任何要求配置，最好还包括 *README* 文件的其他地方的构建说明。

有些层以一个层的形式包含其他 BSP 层。举个这种层的例子就是 *meta-intel* 层，它作为一个层包含了其他 BSP 子层， 还包含名为 *common/* 的目录，它里面全是这些层的公共内容。另外一个例子就是之前提及的 *meta-yocto-bsp* 层。

获取更多层的详细信息，查看 Yocto 项目开发手册的 [“深入理解并创建层”](#understanding-and-creating-layers) 章节。

<a id="preparing-your-build-host-to-work-with-bsp-layers">1.2. 准备您用于开发 BSP 层的编译主机</a>
=======

这段介绍如何准备您的编译主机来开发 BSP 层。只要你配置好了你的主机环境，你就可以创建段落 “[使用 yocto-bsp 脚本创建一个 BSP 层](#creating-a-new-bsp-layer-using-the-yocto-bsp-script)” 描述的层了。

> 提示
>
> 想获取 BSP 的结构信息, 请查看 [文件系统布局的例子](#bsp-filelayout) 段落。

- **1. 配置构建环境**：确保要在命令行下用 BitBake 设置。查看 Yocto 项目开发任务手册的 [“配置 Yocto 项目主机开发环境”](#setting-up-the-development-host-to-use-the-yocto-project) 章节可获取如何得到一个构建主机的信息，可以是本地 Linux 机器或使用 [CROPS][3] 工具容器化的机器。
- **2. 克隆 Poky 仓**：你本地需要一个 Yocto 项目的源码[拷贝目录](#source-directory)(如: 一个 poky 仓拷贝)。查看 Yocto 项目开发任务手册的[“克隆 poky 仓”](#cloning-the-poky-repository)也可能是[“从 poky 检出分支”](#checking-out-by-branch-in-poky)和 ["从 poky 检出tag"](#checkout-out-by-tag-in-poky)段落，来获取如何克隆 poky 仓并检出合适的工作拷贝的相关信息。
- **3. 定制您的 BSP 层**：Yocto 项目支持很多 BSP 层，每个层维护一个 或多个 BSP 机型。想知道 BSP层支持机型的信息，可在官方发布的 [机型索引][4]查看。
- **4. 克隆可选的 BSP 层 meta-intel**：如果你的硬件是英特尔的 CPU 和外设，你可以使用这个 BSP 层。获取 BSP 层 *meta-intel* 的更多信息，请查看层的自述文件 [README][5]
   - a. **索引到您的源码目录**： 通常您需要配置 *meta-intel* 到[源码目录](#source-directory)里。（例如：poky）
   - b. **克隆层**：
 ```
      $ git clone git://git.yoctoproject.org/meta-intel.git
     Cloning into 'meta-intel'...
     remote: Counting objects: 14224, done.
     remote: Compressing objects: 100% (4591/4591), done.
     remote: Total 14224 (delta 8245), reused 13985 (delta 8006)
     Receiving objects: 100% (14224/14224), 4.29 MiB | 2.90 MiB/s, done.
     Resolving deltas: 100% (8245/8245), done.
     Checking connectivity... done.
 ```
   - c. **检出合适的分支**：检出 meta-intel 分支必须与 Yocto 项目正式的分支名匹配（例如：rocko）：

 ```
     $ git checkout branch_name
 ``` 

        如何找出分支名并检出分支的例子，请查看 Yocto 项目开发手册的 [“在 Poky 中检出分支”](#checking-out-by-branch-in-poky) 段落。
- **5. 可选择的配置一个替代的 BSP 层**：如果你的硬件很接近已知的 BSP 层，但又不在 BSP 层 meta-intel 中，你可以克隆它：  
这么做的目的是用来区别于 meta-intel 层名字。例如：如果你使用接近 meta-minnow 的层，克隆层过程如下：
 ```
     $ git clone git://git.yoctoproject.org/meta-minnow
     Cloning into 'meta-minnow'...
     remote: Counting objects: 456, done.
     remote: Compressing objects: 100% (283/283), done.
     remote: Total 456 (delta 163), reused 384 (delta 91)
     Receiving objects: 100% (456/456), 96.74 KiB | 0 bytes/s, done.
     Resolving deltas: 100% (163/163), done.
     Checking connectivity... done.
  ```
- **6. 初始化构建环境**：在源码根目录（如：poky）运行环境配置脚本 [oe-init-build-env][6] ，用来设置编译主机的环境变量：
 ```
 $ source oe-init-build-env
 ```
 除此之外，此脚本创建了[编译目录][7]，即本例子的 build 目录，它在[源码根目录][8]，当运行脚本后你的工作目录就被设置为 build 目录
via:https://www.yoctoproject.org/docs/2.4/bsp-guide/bsp-guide.html

作者：[Scott Rifenbark](mailto:srifenbark@gmail.com)
译者：[巴龙](https://github.com/guevaraya)
校对：[校对者ID](https://github.com/校对者ID)
版权所有© Linux 基金会





[1]: mailto:srifenbark@gmail.com
[2]: http://www.yoctoproject.org/documentation
[3]: https://git.yoctoproject.org/cgit/cgit.cgi/crops/tree/README.md
[4]: http://downloads.yoctoproject.org/releases/yocto/yocto-2.4/machines
[5]: http://git.yoctoproject.org/cgit/cgit.cgi/meta-intel/tree/README
[6]: http://www.yoctoproject.org/docs/2.4/ref-manual/ref-manual.html#structure-core-script
[7]: http://www.yoctoproject.org/docs/2.4/ref-manual/ref-manual.html#build-directory
[8]: http://www.yoctoproject.org/docs/2.4/ref-manual/ref-manual.html#source-directory
