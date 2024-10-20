<a id="using-the-extensible-sdk">第二章 使用可扩展 eSDK</a>
=====

**目录结构**
* [2.1. 为什么要使用可扩展的 SDK 以及里面有些什么](#why-use-the-extensible-sdk-and-what-is-in-it)
* [2.2. 安装可扩展 SDK](#installing-the-extensible-sdk)
  * [2.2.1 两种安装可扩展 eSDK 的方法 ](#two-ways-to-install-the-extensible-sdk)
  * [2.2.2 直接在Yocto构建中配置eSDK环境](#setting-up-the-extensible-sdk-environment-directly-in-a-yocto-build)
  * [2.2.3 通过独立安装包配置eSDK](#setting-up-the-extensible-sdk-from-a-standalone-installer)
* [2.3. 运行可扩展 SDK 环境配置脚本](#23-运行可扩展-sdk-环境配置脚本)
* [2.4.  在你的 SDK 流程中运用 devtool](#24-在你的-sdk-流程中运用-devtool)
* [2.4.1. 用 devtool add 增加一个应用程序](#sdk-use-devtool-to-add-an-application)
* [2.4.2. 用 devtool modify 修改已有的组件源码](#sdk-devtool-use-devtool-modify-to-modify-the-source-of-an-existing-component)
* [2.4.3. 用 devtool upgrade 更新来创建一个新配方，用来兼容软件的新版本](#sdk-devtool-use-devtool-upgrade-to-create-a-version-of-the-recipe-that-supports-a-newer-version-of-the-software)
* [2.5. 进一步理解 devtool 的创建操作](##sdk-a-closer-look-at-devtool-add)
* [2.5.1. 名称和版本号](#sdk-name-and-version)
* [2.5.2. 依赖检查与映射](#sdk-dependency-detection-and-mapping)
* [2.5.3. 许可证检查](#sdk-license-detection)
* [2.5.4. 增加只用 Makefile 编译的软件](#sdk-adding-makefile-only-software)
* [2.5.5. 添加本机工具](#sdk-adding-native-tools)
* [2.5.6. 添加 Node.js 模块](#sdk-adding-node-js-modules)
* [2.6. 如何使用配方（Recipes）](#sdk-working-with-recipes)
* [2.6.1. 查找日志和运行文件](#sdk-finding-logs-and-work-files)
* [2.6.2. 设置配置参数](#sdk-setting-configure-arguments)
* [2.6.3. 配方（Recipe）之间共享文件](#sdk-sharing-files-between-recipes)
* [2.6.4. 打包](#sdk-packaging)

这一章介绍了可扩展 SDK 和如何安装它。具体信息包含 SDK 的组成部分，如何安装它，并概要介绍 devtool 功能展示。可扩展 SDK 使其很容易的添加一个新的应用程序和库文件到镜像，修改现有组件的源码，在目标硬件验证测试，作为附加可轻松集成到 [OpenEmbedded 的编译系统](#build-system-term)上。

>提示
>
>如需比较可扩展 eSDK 和标准 SDK 的主要特性差异，请查看[“介绍”](intro.md#sdk-manual-intro)部分。

除过了通过 devtool 使用其功能外，你可以直接使用工具链，例如 Makefile,Autotools。请查看[“直接使用 SDK 工具链”](working-projects.md#using-the-sdk-toolchain-directly)章节获取更多信息。


<a id="why-use-the-extensible-sdk-and-what-is-in-it">2.1. 使用可扩展 eSDK 的原因以及里面有些什么?</a>
=====

可扩展 SDK 提供提供了交叉编译环境的工具链和为特定镜像定制的库文件。如果你想拥有使用带有强大的 devltool 命令工具链的经验，而这些命令都是为 Yocto 项目环境定制的，你需要用可扩展 SDK。

安装的可扩展 SDK 包含了几个文件和目录。基本上说，它包含 SDK 环境配置脚本，一些设置文件，一个内部编译系统以及 devtool 的功能。


<a id="installing-the-extensible-sdk">2.2. 安装可扩展 eSDK</a>
=====

<a id="two-ways-to-install-the-extensible-sdk">2.2.1 两种安装可扩展 eSDK 的方法 </a>
======
可扩展eSDK有两种不同的安装方式，各有利弊:
1. **直接在Yotco构建中配置可扩展eSDK** 这个可以避免不得不构建，测试，分发和维护多个SDK安装包，这个会特别庞大。这种方式只有一个常规的Yocto构建环境和SDK，少量的根据配置与否来编译的代码路径。这种方式更新SDK也很方便：使用git fetch或层管理工具集来很容易的更新Yocto 层文件。SDK的扩展性也比后面一种的方式好用：仅运行 ``bitbake`` 来增加更多的事情到 sysroot 或增加更多想要的层文件
2. **从独立安装包配置eSDK** 这个方法对有一个单独的且包含了所有需求的可执行制作包。也就是说不需要重新编译，也不需要为低性能笔记本在网上提供功能完善的可执行制作包的缓存
。

<a id="setting-up-the-extensible-sdk-environment-directly-in-a-yocto-build">2.2.2 直接在Yocto构建中配置eSDK环境</a>
======
1. 配置所有需要的层文件和 Yocto 构建环境，例如：在常规 Yocto 构建执行``bitbake`` .
2. 执行如下：
```
$ bitbake meta-ide-support
$ bitbake -c populate_sysroot gtk+3
# 其他任何需要的目标或者本地选项，这是应用程序开发者所需要的
$ bitbake build-sysroots -c build_native_sysroot && bitbake build-sysroots -c build_target_sysroot
```


<a id="setting-up-the-extensible-sdk-from-a-standalone-installer">2.2.3 通过独立安装包配置eSDK</a>
======
首先你需要做的事情是通过运行 ``*.sh`` 安装脚本在[编译主机][25]上安装 SDK。

你可以下载 tar 格式的安装包，它包含预编译的工具链，用于``runqemu``模拟器的脚本，内部编译系统，``devtool`` 和支持文件，这些文件在发行版索引里对应的[工具链][26]目录里。工具链支持多个32位和 ``x86_64`` 目录下的64位架构体系。Yocto 项目提供的工具链是基于 ``core-image-sato`` 和 ``core-image-minimal`` 镜像，它还包含了用于开发此镜像的库文件。

安装包脚本的名字的前缀代表了主机系统，接着后面的字符串代表体系架构的类型。可扩展 SDK 使用字符串“-ext”代表。下面的例子是典型：

```
     poky-glibc-host_system-image_type-arch-toolchain-ext-release_version.sh

     源自:
         host_system 对应的字符串代表主机开放系统的体系架构：

                    i686 or x86_64.

         image_type 代表 SDK 编译出来的镜像名称：

                    core-image-sato or core-image-minimal

         arch 代表正在调测的目标架构体系：

                    aarch64, armv5e, core2-64, i586, mips32r2, mips64, ppc7400, or cortexa8hf-neon

         release_version Yocto 项目发布版本号

                    5.0.2, 5.0.2+snapshot
```

例如，下面的 SDK 安装包是在64 位主机系统开发调测 i586 目标体系架构，用最新的 5.0.2 来编译出 core-imge-sato 镜像：
```
poky-glibc-x86_64-core-image-sato-i586-toolchain-ext-5.0.2.sh
```
    
            
> 提示
>
>可二选一的下载 SDK，也可编译 SDK 安装包。如需获取安装包编译的信息，可查阅[“编译安装包”](appendix-obtain.md#building-an-sdk-installer)部分。

SDK 和工具链都是内置的，默认都被安装在用户目录的 poky_sdk 目录。在安装的时候你可以选择把可扩展 SDK 安装在任意地方。但是需求其文件在正常操作中确保可被写入，安装目录也对使用 SDK 的用户要有写入权限。

下面的命令展示了如何运行64位x86的主机系统下构建64位x86体系架构的安装包。这个例子假定 SDK 安装包位于 `~/Download/` 并有可执行权限。


```
     $ ./Downloads/poky-glibc-x86_64-core-image-minimal-core2-64-toolchain-ext-2.5.sh
     Poky (Yocto Project Reference Distro) Extensible SDK installer version 2.5
     ==========================================================================
     Enter target directory for SDK (default: ~/poky_sdk):
     You are about to install the SDK to "/home/scottrif/poky_sdk". Proceed [Y/n]? Y
     Extracting SDK..............done
     Setting it up...
     Extracting buildtools...
     Preparing build system...
     Parsing recipes: 100% |##################################################################| Time: 0:00:52
     Initialising tasks: 100% |###############################################################| Time: 0:00:00
     Checking sstate mirror object availability: 100% |#######################################| Time: 0:00:00
     Loading cache: 100% |####################################################################| Time: 0:00:00
     Initialising tasks: 100% |###############################################################| Time: 0:00:00
     done
     SDK has been successfully set up and is ready to be used.
     Each time you wish to use the SDK in a new shell session, you need to source the environment setup script e.g.
      $ . /home/scottrif/poky_sdk/environment-setup-core2-64-poky-linux
```
>小贴士
>
>如果你对 SDK 的安装目录没有写权限，安装包会提示给你然后退出。这种情况需要你添加合适的权限然后再运行安装包。

            
<a id="running-the-extensible-sdk-environment-setup-script">2.3. 运行可扩展 SDK 环境配置脚本</a>
=====

一旦你已经安装了 SDK，在实际使用 SDK 之前就必须先运行 SDK 环境配置脚本。

当你在 Yocto 构建中使用一个 SDK 目录，你可以看到在你的[构建目录 Build Directory][27] 看到``tmp/deploy/images/qemux86-64/`` 

当使用独立安装包的时候，这个配置脚本所在目录是在安装 SDK 时选择的安装目录，要么是 ``poky_sdk`` 的默认目录或者是某个安装过程中选择的。

在运行脚本之前，需要确保对应的体系架构是我们即将要开发的。环境配置脚本以字符串“environment-setup”开头，并包含要调试的体系架构名称。例如，下面命令跳转到 SDK 安装目录然后 source 执行环境配置脚本。这个例子的配置脚本用 i586架构体系调试开发 IA架构体系。

```
$ cd /home/scottrif/poky_sdk
$ source environment-setup-core2-64-poky-linux
SDK environment now set up; additionally you may now run devtool to perform development tasks.
Run devtool --help for further details.
```
如果在 一个 Ycoto 构建使用环境脚本的话，可以类似的执行下面命令：
```
$ source tmp/deploy/images/qemux86-64/environment-setup-core2-64-poky-linux
```
运行的配置脚本定义了很多环境变量，他们用来更好的使用 SDK（如：``PATH``，[CC][17]，[LD][18] 等等）。如果你想查看脚本所输出的所有的环境变量，研读安装脚本文件本身就可以了。

<a id="using-devtool-in-your-sdk-workflow">2.4 在 SDK 工作流中如何使用 devtool</a>
=====

可扩展 SDK 的基础工具是 devtool 的命令行工具。这个工具提供了很多有用的特性：在可扩展 SDK 里编译，测试和软件打包，也可以通过 OpenEmbedded 编译系统集成到镜像里。

> 小贴士
>
>对使 devtool 的使用不局限于可扩展 SDK ，可以用 devtool 帮你容易的开发任何项目，将这些项目编译出一个镜像的一部分。

devtool 命令行的组织方式与 [Git][19]的子命令的功能方式很像。你可以运行 devtool --help 查看所有的命令。
> 查看 Yocto 项目参考手册的["devtool 快速参考"][20]获取 devtool 的快速参考。

下面三个 devtool 的子命令提供了开发的入口点：
* _devtool add:_ 辅助添加需要编译的新软件
* _devtool modify:_ 配置一个环境可以修改的已有组件源码
* _devtool ide-sdk:_ 为 IDE 生成一个配置
* _devtool upgrade:_ 更新已有的配方以便编译更新一套源文件。

在编译系统里，“配方（recipes）”代表 devtool里的软件包。当执行 devtool add 后，一个配方被自动创建。当你执行 devtool modify 命令后，指定的现有配方用来决定从哪里获取源码以及如何打补丁。这两个情况下，配置好环境是为了让你编译想要修改的配方源码。新配方及其源码默认都在 SDK 的 “workspace” 目录下。

这个段落的后面主要描述 devtool add， devtool modify ，_devtool ide-sdk和 devtool upgrade 的执行流程。

<a id="use-devtool-add-to-add-an-application">2.4.1. 使用 devtool add 添加一个应用程序</a>
======
devtool add 命令可在已有的源代码基础上生成一个配方。这个命令利用了[工作目录层][21]生产的，其他很多 devtool 命令也是这么用的。这个命令很灵活的帮你将源码解压到工作目录或一个特定的本地 Git 仓。如果用的是已经存在的代码就不需要解压。

根据特定的需求场景，devtool add 的参数和选项形成不同的组合。下面图表展示了使用 devtool add 命令可能用到的常见开发流程：

![sdk-devtool-add-flow][22]

1. **生成新配方**:  流程图的顶部展示了用 devtool add 基于现有的源码生成配方的三个场景。

在共享开发环境下，通常其他开发者负责不同的代码开发。作为一个开发者，你可能关心如何在 Yocto 项目的开发中使用部分代码。你需要是能获取源码，配方以及完成工作的可操作区域。
这个图表中，三个可能的场景囊括在 devtool add 的流程里：
* **左边**：图表最左边的场景代表常见的源码不在本地需要解压的情况。这种情况下源码解压在默认的工作目录-你不需要工作目录之外的特定目录文件。因此所有你需要的东西都在工作目录：
```
$ devtool add recipe fetchuri
```
用这个命令，devtool 解压上游源码文件到本地的 Git 仓到 ``sources`` 目录。然后这个命令会在工作目录创建一个名叫 recipe 的配方和一个对应的 append 配方文件。如果没有提供配方名称，命令会尝试自己生成一个。
* **中间**：图表里的中间场景也代表源码不在本地的情况。这样的话，代码同样在上游并需要解压到本地，其中一些目录不在默认的工作目录。

>提示
>
> 如果需要，devtool 在解压的过程中总是会在本地创建一个 Git 仓。
进一步说，这个例子里的第一个位置参数 srctree 定义了devtool add 命令在工作量目录之外的解压目录。它需要指定一个空目录：
```
$ devtool add recipe srctree fetchuri 
```
总之，从 fetchuri 获取源码然后作为 Git 代码仓解压到 srctree 本地目录。
在工作目录里，devtool 创建一个名称为recipe和带有append 的配方文件。

* **右边：** 图表右边的场景代表 srctree 在 devtool 工作目录之外已提前配置的情况。

下面命令创建一个名为 recipe的新配方，并能识别现有的源码：
```
$ devtool add recipe srctree
```
这个命令检查源码并为源码创建一个叫 recipe 的配方然后将其放到工作目录。
由于解压的源码已经存在，devtool 不需要尝试搬运源码到工作目录-仅在工作目录需要一个新配方。
配方目录里，该命令同时创建了一个包含 *.bbappend 文件的 append 相关目录。

2. **配方编辑：** 你可以用 devtool edit-recipe 命令用 $EDITOR 变量定义的编辑器修改配方文件：
```
$ devtool edit-recipe recipe
```
在编辑器里对配方的编辑修改将会在后面的构建中生效。
3. **编译配方或重建镜像：** 下一步的动作取决于如何处理新的代码。
如果你需要最终会将编译物移动到目标硬件的目录，那就用下面的 devtool 命令：
```
$ devtool build recipe
```
另一方面，如果你想将包含配方包的镜像从工作目录打包发布到设备上（例如：用于测试目录），你可以用 devtool build-image 命令：
```
$ devtool build-image image
```
4. ** 部署编译生成物： ** 当你用 devtool build 命令编译你的配方时，你可能想看到编译生成物在目标硬件上的运行效果。

> 提示
>
> 这个步骤是假定你已经有了一个可以在 QEMU 或 实际硬件上运行的构建镜像。也假定镜像部署到了目标板上，并安装了 SSH，如果镜像运行在实际的硬件上，你也有从开发主机访问的网络权限。

你可以用 devtool deploy-target 命令部署编译输出物到目标硬件：
```
$ devtool deploy-target recipe target
```
target 是一个作为一个 SSH 服务器运行的目标机器。
你也可以通过 devtool build-image 命令部署编译的镜像到实际硬件。但是 devtool 不能提供特定的命令让你部署镜像到实际硬件。

5. **使用配方收尾你的工作：** devtool finish 一般会创建对应的补丁到本地 Git 仓，生成一个新配方将其移动到一个更稳定的层里，然后重置配方以便在工作目录之外可正常编译。
```
 $ devtool finish recipe layer
```

>小提示
>
>任何你要生成的补丁必须要提交到源码目录的 Git 仓。

如上所，devtool finish 移动最终的配方到它的稳定层。
devtool finish 命令作为最终的工序，恢复标准层和上游源码的状态以便在这些区域构建配方而不是在工作目录。

>小提示
>
>当你不想继续工作，你可以用 devtool reset 命令恢复的工作区域。如果你用了这个命令，请记住源码树还是保留的。

2.4.2. 使用 devtool modify 修改源码的已有组件
======

``devtool modify``命令用来如何在已有代码上做准备工作，然后用已有的配方来构建软件。此命令很方便的让你从上游解压源码，保持对其跟踪并收集从其他开发者来的补丁文件。
根据你的特定需求，你可以选择``devtool modify``命令的参数和选项形成不同的组合。下面图表展示了``devtool modify``命令的不同的组合：

![使用 devtool modify 修改源码的已有组件][23]

1. **准备修改源码**： 流程图最上面展示了三个应用场景：我们可以用 devtool modify  命令在源码文件做一些准备工作。每个场景有以下前提假设：
   * 配方(recipe)在 ``devtool`` 的本地工作目录的外部层（layer）已经存在。
   * 源码文件在上游压缩形式存在或在以解压展开状态在本地。
 
      进一步说，他们的源码文件既可以是远程上游的也可是本地的。典型的情况是其他开发者为使用 Yocoto Project 和它们的自己本地的配方而创建一个层。  
   * **左边**：图标最左边的场景代表源码不在本地的常见的情况。它需要从上游源码解压。这种情况，源码被解压到默认的``devtool``的本地工作目录里。这个场景的配方（recipe）就是他的工作目录外面的层（layer）文件。（例如：meta-layername）
  
      下面命令定义配方默认并解压源码文件：

      ```
      devtool modify recipe
      ```


      一旦``devtool``找到配方（recipe），``devtool``用配方的 [SRC_URI][24] 变量找到源码和其他开发者提供的本地补丁。

      这个场景下参数``srctree``为空。这样，``devtool modify``命令的默认行为就是解压源码到[SRC_URI][24] 参数指定的本地 Git 目录。也就是说， 本地解压的源码目录就是 devtool 默认工作目录。结果是该命令在工作目录配置了源码和附件配方文件，而配方（recipe）还保留在它原来的路径。

      另外，如果你有任何本地非补丁文件（例如：在 SRC_URI 语句包含``file://``引用文件，但不包括``*.patch/``或 ``*.diff``），这个文件被拷贝到新创建源码树的一个``oe-local-files``的目录。拷贝文件到这个地方后可以让我们便捷在这修改文件。您对这些文件的任何修改和添加都会在下一次构建中体现，就像你对源码的任何修改的效果一样。

   * **中间**：
      上图里中间的场景代表源码在本地不存在的情况，这时代码同样在上行仓库需要以Git仓的形式解压到某个本地区域。这个情况下的菜谱（recipe) 也是在本地工作目录之外的一个层目录里。
      下面的命令告诉 ``devtool``需要操作的菜谱（recipe)，同时制定本地即将解压源码的目录，这个目录在 ``devtool`` 默认工作目录之外:
      ```
      $ devtool modify recipe srctree
      ```

      > 小贴士
      > 
      > devtool中的``srctree``不能为URL地址

      所有解压操作中，devtool命令从菜谱（recipe)的[SRC_URI][24] 变量中得到源码和响应的补丁文件。非补丁文件拷贝到基于新创建源码目录的``oe-local-files``目录。 
      一旦源码文件被确认，devtool命令默认加压到``srctree``
      devtool 为菜谱（``recipe``）创建一个追加文件。菜谱（``recipe``）保持在原生的位置但源码解压到指定的 srctree 目录。

   * **右边**：

原文: https://docs.yoctoproject.org/5.0.2/sdk-manual/extensible.html

译者：[巴龙](https://github.com/guevaraya)
校对：[校对者ID](https://github.com/校对者ID)
版权所有© 2010-2019 Linux 基金会

[1]: https://creativecommons.org/licenses/by-sa/2.0/uk/
[2]: http://www.yoctoproject.org/documentation
[3]: https://wiki.yoctoproject.org/wiki/Releases
[4]: http://www.yoctoproject.org/docs/5.0.2/ref-manual/ref-manual.html#build-system-term
[5]: http://www.yoctoproject.org/docs/5.0.2/ref-manual/ref-manual.html#var-CC
[6]: http://www.yoctoproject.org/docs/5.0.2/ref-manual/ref-manual.html#var-LD 
[7]: http://www.yoctoproject.org/docs/5.0.2/ref-manual/ref-manual.html#cross-development-toolchain
[8]: http://www.yoctoproject.org/docs/5.0.2/ref-manual/ref-manual.html#build-directory
[9]: http://www.yoctoproject.org/docs/5.0.2/ref-manual/ref-manual.html#source-directory
[10]: https://www.yoctoproject.org/docs/5.0.2/sdk-manual/figures/sdk-environment.png
[11]: https://www.yoctoproject.org/docs/5.0.2/sdk-manual/sdk-manual.html#sdk-installing-the-sdk
[12]: http://downloads.yoctoproject.org/releases/yocto/yocto-5.0.2/machines
[13]: http://downloads.yoctoproject.org/releases/yocto/yocto-5.0.2/machines/qemu
[14]: https://www.yoctoproject.org/docs/5.0.2/sdk-manual/sdk-manual.html#sdk-extracting-the-root-filesystem
[15]: http://wiki.qemu.org/Main_Page
[16]: http://www.yoctoproject.org/docs/5.0.2/dev-manual/dev-manual.html#dev-manual-qemu
[17]: http://www.yoctoproject.org/docs/5.0.2/ref-manual/ref-manual.html#var-CC
[18]: http://www.yoctoproject.org/docs/5.0.2/ref-manual/ref-manual.html#var-LD
[19]: https://www.yoctoproject.org/docs/5.0.2/overview-manual/overview-manual.html#git
[20]: https://docs.yoctoproject.org/5.0.2/ref-manual/devtool-reference.html
[21]: https://docs.yoctoproject.org/5.0.2/ref-manual/devtool-reference.html#devtool-the-workspace-layer-structure
[22]: https://docs.yoctoproject.org/5.0.2/_images/sdk-devtool-add-flow.png
[23]: https://docs.yoctoproject.org/5.0.2/_images/sdk-devtool-modify-flow.png
[24]: https://docs.yoctoproject.org/5.0.2/ref-manual/variables.html#term-SRC_URI
[25]: https://docs.yoctoproject.org/5.0.2/ref-manual/terms.html#term-Build-Host
[26]: https://downloads.yoctoproject.org/releases/yocto/yocto-5.0.2/toolchain/
[27]: https://docs.yoctoproject.org/5.0.2/ref-manual/terms.html#term-Build-Directory