
2 使用可扩展 SDK
====

这一章介绍了可扩展 SDK 以及如何安装它。详细内容包含 SDK的组成部分，如何安装它，并概要介绍 devtool 功能展示。可扩展 SDK 使其很容易的添加一个新的应用程序和库文件到镜像，修改现有组件的源码，在目标硬件验证测试，作为附件可轻松集成到 [OpenEmbedded 的编译系统](../ref-manual/terms.md#嵌入式编译系统openembedded-build-system)上。

>提示
>
>要比较可扩展 SDK 和标准 SDK 的主要特性，请查看[“介绍”](intro.md#1-介绍)部分。

除过通过 devtool 使用其功能外，你可以直接使用工具链，例如从 Makefile 到 Autotools。请查看[“直接使用 SDK 工具链”](working-projects.md#使用-sdk-的交叉工具链目录)章节获取更多信息。

2.1. 为什么要使用可扩展的 SDK 以及里面有些什么
=====

可扩展 SDK 提供提供了交叉编译环境的工具链和为特定镜像定制的库文件。如果你想拥有使用带有强大的 devltool 命令工具链的经验，而这些命令都是为 Yocto 项目环境定制的，你需要用可扩展 SDK。

安装的可扩展 SDK 包含了几个文件和目录。基本上说，它包含 SDK 环境配置脚本，一些设置文件，一个内部编译系统以及 devtool 的功能。


2.2. 安装可扩展 SDK
=====

首先你需要做的事情是通过运行 \*.sh 安装脚本在[编译主机](../ref-manual/terms.md#build-host)上安装 SDK。

你可以下载 tar 格式的安装包，它包含预编译的工具链，用于运行 QEMU 模拟器的脚本，内部编译系统，devtool 和配套文件，这些文件在发行版索引对应的工具链目录里。[工具链][1]持多个32位和 x86_64 目录下的64位架构体系。Yocto 项目提供的工具链是基于 core-image-sato 和 core-image-minimal 镜像，它还包含了用于开发此镜像的库文件。

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

                     3.4.1, 3.4.1+snapshot
```

例如，下面的 SDK 安装包是在64 位主机系统开发调测 i586 目标体系架构，用最新的 3.4.1 来编译出 core-imge-sato 镜像：
```
poky-glibc-x86_64-core-image-sato-i586-toolchain-ext-3.4.1.sh
```
    
            
> 提示
>
>可二选一的下载 SDK，也可编译 SDK 安装包。获取安装包编译的信息，可查阅[“编译安装包”](appendix-obtain.md#52-编译一个sdk的安装器)部分。

SDK 和工具链都是内置的，默认都被安装在用户目录的 poky_sdk 目录。在安装的时候你可以选择把可扩展 SDK 安装在任意地方。但是要求其文件在正常操作中确保可被写入，安装目录也对使用 SDK 的用户要有写入权限。

下面的命令展示了如何运行64位x86的主机系统下构建64位x86体系架构的安装包。这个例子假定 SDK 安装包位于 `~/Download/` 并有可执行权限。

>提示
>
>如果你对 SDK 的安装目录没有写权限，安装包会提示然后退出。这种情况需要你添加合适的权限然后再运行安装包。


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
            
2.3. 运行可扩展 SDK 环境配置脚本
=====

一旦你已经安装了 SDK，在实际使用 SDK 之前就必须先运行 SDK 环境配置脚本。这个配置脚本所在目录使在安装 SDK 时选择的，如果不是 poky_sdk 的默认目录就是你选择的安装目录。

在运行脚本之前，需要确保对应的体系架构是我们即将要开发的。环境配置脚本以字符串“environment-setup”开头，并包含要调试的体系架构名称。例如，下面命令跳转到 SDK 安装目录然后 source 执行环境配置脚本。这个例子的配置脚本用 i586架构体系调试开发 IA架构体系。

```
     $ cd /home/scottrif/poky_sdk
     $ source environment-setup-core2-64-poky-linux
     SDK environment now set up; additionally you may now run devtool to perform development tasks.
     Run devtool --help for further details.
```

运行配置脚本定义了很多环境变量，他们可以更好的使用 SDK（如：PATH，[CC](../ref-manual/variables.md#cc)，[LD](../ref-manual/variables.md#cc) 等等）。如果你想查看脚本所输出的所有环境变量，研读下安装文件本身就可以了。

2.4. 在你的 SDK 流程中运用 devtool
=====

可扩展 SDK 的基石是一个名叫 devtool 的命令行工具。这个工具提供了很多有用的特性：在可扩展 SDK 里编译，测试和软件打包，也可以通过 OpenEmbedded 编译系统集成到镜像里。

> 小贴士
>
>对 devtool 的使用不局限于可扩展 SDK ，可以用 devtool 帮你容易的开发任何项目，这些项目都能编译出镜像的一部分。

devtool 命令行的组织方式与 [Git](../overview-manual/development-environment.md#git)的子命令的功能方式很像。你可以运行 devtool --help 查看所有的命令。
> 查看 Yocto 项目参考手册的["devtool 快速参考"](../ref-manual/devtool-reference.md#7-devtool-快速参考)获取 devtool 的快速参考。

下面三个 devtool 的子命令提供了开发的入口点：
* _devtool add:_ 辅助添加需要编译的软件
* _devtool modify:_ 配置一个环境让你可以修改已有的组件源
* _devtool upgrade:_ 更新已有的配方以便编译更新一套源文件。

在编译系统里，“配方（recipes）”代表 devtool里的软件包。当执行 devtool add 后，一个配方被自动创建。当你执行 devtool modify 命令后，指定的现有配方用来决定从哪里获取源码以及如何打补丁。这两个情况下，配置好环境是为了让你编译想要修改的配方源码。新配方及其源码默认都在 SDK 的 “workspace” 目录下。

这个段落的后面主要描述 devtool add， devtool modify 和 devtool upgrade 的执行流程。

2.4.1. 使用 devtool add 添加一个应用程序
======
devtool add 命令可在已有的源代码基础上生成一个配方。这个命令对很多 devtool 所生成的[工作目录的Layer结构](../ref-manual/devtool-reference.md#72-工作目录的layer结构)都很有用。这个命令很灵活的帮你将源码解压到工作目录或一个特定的本地 Git 仓。如果用的是已经存在的代码就不需要解压。

根据特定的需求场景，devtool add 的参数和选项形成不同的组合。下面图表展示了使用 devtool add 命令可能用到的常见开发流程：

![sdk-devtool-add-flow][2]

1. **生成新配方**:  流程图的顶部展示了用 devtool add 基于现有的源码生成配方的三个场景。

在共享的开发环境下，通常其他开发者各自负责各种代码。作为一个开发者，你可能关心如何在 Yocto 项目的开发中使用代码。你需要获取源码，配方以及完成工作的可操作区域。
这个图表中，三个可能的场景囊括在 devtool add 的流程里：
* **左边**：图表最左边的场景代表常见的源码不在本地需要解压的情况。这种情况下源码解压在默认的工作目录-你不需要工作目录之外有其它特定目录文件。因此所有你需要的东西都在工作目录里：
```
$ devtool add recipe fetchuri
```
用这个命令，devtool 解压上游源码文件到本地的 Git 仓源码目录。然后这个命令会在工作目录创建一个名叫 recipe 的配方和一个对应的 append 配方文件。如果没有提供配方名称，命令会尝试自己生成一个。
* **中间**：图表里的中间场景也代表源码不在本地的情况。这样的话，代码同样在上游并需要解压到本地，其中一些目录放不在默认的工作目录。

>提示
>
> 如果需要，devtool 在解压的过程中总是会在本地创建一个 Git 仓。
进一步说，这个例子里的第一个位置参数 srctree 定义了devtool add 命令在工作目录之外要解压目录。它需要指定一个空目录：
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

2. **配方编辑：** 你可以用 devtool edit-recipe 命令也就是用 $EDITOR 变量定义的编辑器修改配方文件：
```
$ devtool edit-recipe recipe
```
在编辑器里对配方的编辑修改将会在后面的构建中生效。
3. **编译配方或重建镜像：** 下一步的动作取决于如何处理新的代码。
如果你需要最终会将编译物移动到目标硬件的目录，那就用下面的 devtool 命令：
```
$ devtool build recipe
```
另一方面，如果你想将包含配方包的镜像从工作目录发布到设备上（例如：用于测试目录），你可以用 devtool build-image 命令：
```
$ devtool build-image image
```
4. ** 部署编译生成物： ** 当你用 devtool build 命令编译你的配方时，你可能想看到编译生成物在目标硬件上的运行效果。

> 提示
>
> 这个步骤是假定你已经有了一个可以在 QEMU 或 实际硬件上运行的构建镜像。也假定镜像部署到了目标板上，并安装了 SSH，如果镜像运行在实际的硬件上，你也可从开发主机通过网络访问。

你可以用 devtool deploy-target 命令部署编译输出物到目标硬件：
```
$ devtool deploy-target recipe target
```
target 是一个作为一个 SSH 服务器运行的目标机器。
你也可以通过 devtool build-image 命令部署编译的镜像到实际硬件。但是 devtool 不能提供定制的命令让你部署镜像到实际硬件。

5. **使用配方收尾你的工作：** devtool finish 一般会创建对应的补丁到本地 Git 仓，生成一个新配方将其移动到一个更稳定的层里，然后重置配方以便在工作目录之外可正常编译。
```
 $ devtool finish recipe layer
```

>小提示
>
> 任何你要生成的补丁必须要提交到源码目录的 Git 仓。

如上所，devtool finish 移动最终的配方到它的稳定层。
devtool finish 命令作为最终的工序，恢复标准层和上游源码的状态以便在这些区域构建配方而不是在工作目录。

>小提示
>
>当你不想继续工作，你可以用 devtool reset 命令恢复的工作区域。如果你用了这个命令，请记住源码树还是保留的。

2.4.2. 使用 devtool modify 修改源码的已有组件
======

devtool modify 命令用来如何在已有代码上做准备工作，然后用已有的配方来构建软件。此命令很方便的让你从上游解压源码，保持对其跟踪并收集从其他开发者来的补丁文件。
根据你的特定需求，你可以选择 devtool modify 命令的参数和选项形成不同的组合。下面图表展示了 devtool modify 命令的不同组合：

![使用 devtool modify 修改源码的已有组件][3]

1. **准备修改源码**： 流程图最上面展示了三个应用场景：我们可以用 devtool modify 命令在源码文件做一些准备工作。每个场景有以下前提假设：
* 配方在 devtool 的本地工作目录的外部层（layer）已经存在。
* 源码文件在上游以压缩形式存在或以解压展开状态在本地。

进一步说，他们的源码文件既可以是远程上游的也可是本地的。
典型的情况是其他开发者为使用 Yocto 项目和它们的自己本地的配方而创建一个层。

* **左边**：图标最左边的场景代表源码不在本地的常见的情况。它需要从上游源码解压。这种情况，源码被解压到默认的 devtool 的本地工作目录里。这个场景的配方（recipe）就是他的工作目录外面的层（layer）文件。（例如：meta-layername）

下面命令定义默认配方并解压源码文件：

```
$ devtool modify recipe
```

一旦 devtool 找到配方（recipe），devtool 通过配方的 SRC_URI 变量找到源码和其他开发者提供的本地补丁。

这个场景下，参数 srctree 为空。因此，devtool modify 命令的默认行为是解压源码到 [SRC_URI][4] 参数指定的本地 Git 目录。也就是说，本地解压的源码目录就是 devtool 默认工作目录。结果是该命令在工作目录配置了源码和附件配方文件，而配方还保留在它原来的路径。

另外，如果你有任何本地非补丁文件（例如：在 SRC_URI 语句包含 file:// 引用文件，但不包括 \*.patch/ 或 \*.diff），这个文件被拷贝到新创建源码树的一个 oe-local-files 的目录。拷贝文件到这个地方后可以让我们便捷在这修改文件。您对这些文件的任何修改和添加都会在下一次构建中体现，就像你对源码的任何修改的效果一样。

* **中间**: 在上图中间的场景代表源码不在本地的情况。该场景下，代码也需要从上游获取并以 Git 仓形式解压到本地目录。该场景下的配方也在本地工作目录之外对应的 layer 里。

下面命令告诉 devtool 会用到那个配方，这个场景明确了工作目录之外的外部源码文件的本地位置：
```
$ devtool modify recipe srctree
```

> 提示
>
> 你不能用 URL 地址作为devtool命令的 srctree 参数

和所有提取动作一样，命令用配方的SRC_URI 参数确定本地源码文件和任意补丁文件。非补丁文件被拷贝到先创建的源码树下的 oe-local-files 目录。
一旦文件被找到，命令默认解压他们到 srctree 目录。
在工作区，devtool 为配方创建 append 文件。配方还继续保持在原有的位置但源码文件解压到用srctree提供的目录里。

* **右边**：图中右边的场景代表源码目录已经存在的情况，它存在于前面 devtool 工作区之外的展开的Git目录。在这个例子中，配方也可能存在于它自身的layer之外的目录。
下面命令告诉devtool要用配方，用“-n”选项代表源码不需要解压，用 srctree 指定之前目录已经展开的源码文件：
```
$ devtool modify -n recipe srctree
```
如果 oe-local-files 子目录存在，它包含了非补丁文件，则它是有用的文件。另外，如果子目录不存在，你可以运行 devtool finish 后，任何在配方同目录的非补丁文件会被移除因为devtool会认为这些文件需要删除。
一旦 devtool modify 命令执行，它会为 devtool 工作区的配方创建一个 append 文件。配方和源码保持在它们原有的位置。

2. **编辑源码**：一旦你使用了 devtool modify 命令，你可以任意的修改源码文件，可以用任何你喜欢的编辑器修改和保存你的源码。

3. **编译配方或重新编译镜像**：下一步的动作就是如何使用新代码。
如果你最后需要将输出物放到目标硬件上，用下面命令：
```
$ devtool build recipe
```
另外，如果你想将现编的包含配方包的镜像里放到设备里（例如：为了测试验证），你可以用 devtool build-image 命令：
```
$ devtool build-image image
```
4. **部署编译输出物**：如果你用devtool build 命令编译配方，你肯定想知道编译出来能不能在目标硬件上执行。

>提示
>
> 这一步假定前面编译的镜像已经在QEMU或实际的硬件上运行。同时全部部署在了目标设备上，ssh 已经被安装在镜像里，如果镜像运行在实际的硬件上，你需要有开发机器的网络存取权限。
你可以用 devtool deloy-target 命令部署你的输出物到目标设备上：
```
$ devtool deloy-target recipe target
```
目标机器运行着 SSH 服务。
你当然也可以用其他的方法（devtool build-image）部署镜像到实际设备。devtool 不提供定制命令来部署镜像到实际硬件上。

5. **用配方完成你的工作**：devtool finish 创建本地 Git 仓的 commit 对应的patch,基于原有配方更新（基于指定的目标Layer创建.bbapend 文件），然后重置配方确保原有的配方在工作区能正常编译。
```
$ devtool finish recipe layer
```
>提示
>
> 在执行devtool finish命令之前任何你想要的修改必须保存和提交到本地Git仓

因为没必要移动配方，devtool finish 命令要么更新原有的Layer的原始配方要么在新Layer创建一个新的.bbappend 文件。在devtool finish命令执行过程中任何你在 oe-local-files 目录的操作被保留在配方的原始文件里。
当你最终执行了devtool finish命令，标准Layer的状态和上有代码被恢复以便你可以从这些区域编译配方而不是工作区。
>提示
>
> 你可以用devtool reset命令将你不需要的工作内容丢掉。如果你确定要用这个命令，请将源码提前保存好。

2.4.3. 使用 devtool upgrade 来创建一个软件最新版本的配方
======
devtool upgrade 命令更新已有的配方为上游较新的版本。整个软件的生命周期里，配方不停的经历上游发布商的版本更新。你可以通过devtool upgrade 流程确保你使用的配方与最新的上游软件是匹配的。
> 提示
> 
> 有多个方法可以更新配方，devtool upgrade只是其中之一。你可以在 Yocto 项目开发任务手册的[更新配方][5]段落阅读更新配方的所有方法。

devtool upgrade 命令可很方便的指定源码版本，版本管理类型，代码展开位置或devtool的[Layer结构的工作区][6]，任何[Fetchers][7]可以支持的源码格式
下面命令展示了devtool upgrade 命令常见的开发流程：
![sdk-devtool-upgrade-flow][8]
1. **初始化更新**：流程顶部展示了devtool upgrade的典型场景。包含下面几个条件:
* 配方存在于devtool工作区之外的本地layer。
* 新版本的源码位置由配方里的SRC_URI指定（例如：新版本的tar包或在上游的Git仓）
常见的情况是第三方软件对应的软件需要更新版本。你可以获取的配方很有可能在你的layer里。因此，你需要用最新的版本更新配方：
```
$ devtool upgrade -V version recipe
```
缺省情况下devtool upgrade 解压源码到[Layer结构的工作区][6]的soure目录。如果你想解压源码到其他位置，你需要参照如下指定命令的srctree位置参数：
```
$ devtool upgrade -V vesion recipe srctree
```
>提示
>
>这个例子中“-V”指定新版本。如果未使用“-V”，命令更新配方到最新版本。
如果在配方的SRC_URI参数指定了源码文件在Git仓中，你必须指定“-S”选项指定软件的版本。
一旦devtool找到配方，它会用SRC_URI变量从其他开发者查找源码和任何本地补丁文件。结果就是配置工作区的源码，新配方和append文件。
另外如果你有任何非补丁文件（例如在SRC_URI变量里以file://开头但不包含*.patch/或*.diff）,这些文件被拷贝到 oe-local-files 文件夹下新创建的目录里。新拷贝的文件可以让你方便的修改文件。任何修改和添加都会在下一次编译镜像体现，和你对修改是一样的效果。
2. **解决任何更新过程中冲突**：更新了新版本的软件后可能会产生冲突。冲突产生主要在于配方里SRC_URI指定的补丁文件与新版本软件修改之间。例如这种情况，你需要解决编辑源码和git rebase的过程。
在下一步之前，确保通过更新和使用其他不同的软件已经尝试已经解决冲突了。
3. **构建配方或重新编译镜像**：下一步就是你如何处理新代码。
如果你最终是要将编译输出物移挪到目标硬件，请用下面命令：
```
$ devtool build recipe
```
另外，如果你希望镜像里直接包含从工作区部署到设备的配方的包文件（例如用于测试目的），你可以用devtool build-image命令：
```
devtool  build-image image
```

4. **部署编译输出物**：当你用devtool build 命令或bitbake构建配方，你可能像验证编译输出物在目标硬件是否运行正常。
>提示
>
>这一步假定你已经编译的镜像在在QEMU或实际硬件上运行。同时假定可以在目标设备上镜像开发，SSH也已经在设备运行的镜像里安装，并且我们在开发设备上有网络存取权限。
你可以用devtool deploy-target 命令将编译输出物部署到目标硬件上：
```
$ devtool deploy-target recipe target 
```
你也可以用devtool build-image 命令部署编译输出物到实际硬件上。但其定制部署命令devtool是不提供的。
5. **完成配方构建工作**:devtool finish 创建针对Git仓一个对应的补丁，将新的配方移动到一个更持续维护的Layer里，然后复位配方确保其可以正常构建不仅仅是在工作区可以构建。
任何在oe-local-files文件夹的操作在devtool finish命令执行过程中都会被保留到与配方同目录的文件里。
如果指定了与源码同名的目标Layer，然后配方的旧版本和相关的文件被优先移动到新版本里。
```
$ devtool finish recipe layer
```
>提示
>
>如何你需要有用修改的补丁都要提交到源码的Git仓里
作为devtool finish命令的最后一个流程，标准layer和上游代码的状态需要恢复以便能这些区域构建配方而不仅仅在工作区。
>提示
>
>你可以用devtool reset命令回退你不想需要的工作，如果你用这个命令，要确保需要的源码要提前保存好

2.5. 进一步理解 devtool add操作
=====
devtool add 命令根据提供的源码自动创建一个配方。目前，该命令支持以下编译方式：
* Autotools（autoconf和automake）
* CMake
* Scons
* qmake
* 常见的Makefile
* 单独kernel内核模块
* 二进制打包（例如：-b选项）
* Node.js模块
* 用setuptools和disutils的Python模块
除过二进制包，对源码文件的构建方法取决于源码里的文件类型。例如，如果源码里有CMakefiles.txt,则源码被对应认为用CMake构建的。
>提示
>
> 大部分情况下，你需要编辑自动生成的配方文件使其得到正确的构建。典型的，你需要多次编译和来回编译直到配方成功构建。一旦配方可以构建，我们尽可能在目标设备上迭代测试。
该段落后门将详细介绍配方的每一部分是如何生成。根据工具的配置，devtool相应的设置新创建的配方名称。

2.5.1. 名称和版本号
======
如果你在命令行没有指定名称和版本号，devtool add用源码树的几个metadata来确定被编译软件的名称和版本。根据工具的配置，devtool相应的设置新创建的配方的名称
如果devtool 没有确定名称和版本，命令会打印失败。这些情况下，你需要重新运行命令提供名称和版本，将名称和版本作为命令行的一部分。
有时从源码取的名称和版本可能不正确，这种情况下需要重置配方：
```
$ devtool reset -n recipename
```
在运行devtool reset命令之后，你需要提供名称和版本再次运行devtool add命令。

2.5.2. 依赖检查与映射
=======
devtool add 命令尝试检测编译过程中的依赖并与在系统中与其他配方映射关联起来。在映射过程中，改命令将配方的名字填充到配方的[DEPENDS][9]变量里。如果该依赖无法被映射，devtool就在配方中添加一条注释说明此内容。不能映射依赖就会导致从命名上不能识别或者因为简单的依赖也是不可行的。针对依赖不可行的情况，你需要用devtool add 命令额外添加一个配方来满足依赖。一旦你增加一个配方，你需要在原有的配方的[DEPENDS][9]变量里添加包含新的配方。
如果你需要添加运行过程中的依赖，你可以添加以下内容到配方
```
RDEPENDA:${PN} += "dependency1 dependency2 ..."
```
>提示
>
>devtool add 命令经常无法区别必要的依赖和可选依赖。因此，一些检测的依赖可能实际上是可选的。当出现问题，需查阅文档或构建软件对应配方的配置脚本获取更多信息。一些情况，你可以用禁用选项来替代现有的依赖,该选项会传到配置脚本里最终关闭一些相关的功能.

2.5.3. 许可证检查
======

devtool add 命令会尝试确认你添加的软件是否允许在常见的开源许可证下分发。如果可以，命令就会设置 [LICENSE][10] 为相应的值。你一定要仔细检查这个命令生成的这个值，要与编译的软件中的文档或源码匹配，如果有必要，及时更新 [LICENSE][10] 的值。

devtool add 命令同时设置 [LIC_FILES_CHECSUM][11] 的值来代表所有文件的许可证信息。要记住许可证的声明经常出现在源码或文档的前面。而在这个情况下，命令无法识别这些许可证的语句，如果存在许可证的话，你可能需要修改 [LIC_FILES_CHECSUM][11] 变量来指定可能存在这些声明。设置[LIC_FILES_CHECSUM][11]对三方软件来说非常重要。为了将有通过流程来确保上游配方的新版本能更新到正确的许可证。任何许可证的变化都可以被检测到的，当你检测许可证的时候将会收到一个错误提示。

如果devtool add 命令不能确定许可证信息，devtool 可设置 [LICENSE][10] 为 “Closed” 而[LIC_FILES_CHECSUM][11]设置为空。这种改法即使在所有的情况下可能不对但可以不影响开发调试。你需要根据你构建的软件的文档和源码确定实际合适的许可证。

2.5.4. 增加 Makefile-Only 编译的软件
======

使用 Make 编译的方法对私有和开源软件是很场景。不幸的是，Makefile 经常没有考虑交叉编译。这样，devtool add 经常不能确保 Makefile 能够正确的编译。例如，常常需要明确的将gcc用[CC][12]替代。经常在交叉编译环境中，gcc作为编译主机的编译器，交叉编译器有时会命名为 arm-poky-linux-gnueabi-gcc同时还需要一些参数（比如指定目标机器的相关sysroot）。

当需要编写 Makefile-Only 软件，需要注意以下事项：
* 可能需要通过用变量的形式修改Makefile而不是直接硬编码指定这些工具链如gcc或g++
* Make运行的这些环境需要用标准的变量,要与SDK的环境配置脚本相同的环境变量（如：[CC][12]，[CXX][13]等等）。一个简单的查看这些变量的方法就是在配方里运行 devtool build 命令然后查看oe-logs/run.do_compile文件。这个文件的最上面可以看到所涉及的环境变量列表。我们可以查看这些环境变量的值。
* 如果 Makefile 用“=”设置了变量的默认值，这个默认值会覆盖环境变量，最终环境无法生效。遇到这种情况，你可以修改Makefile将默认用“?=”赋值，也可以在make命令行中强制赋值。要是像在命令行强制赋值，需要在配方里增加变量 [EXTRA_OEMAKE][14]  和 [PACKAGECONFIG_CONFARGS][15]。这儿有一个使用 [EXTRA_OEMAKE][14] 变量的例子：
 ```
 EXTRA_OEMAKE += "'CC=${CC}' 'CXX=${CXX}'"
 ```
 上面的例子中，变量里包含了空格变量的要用单引号括起来，而最终这些所需变量需要传递给编译器。
* 在Makefile里的硬编码的路径在交叉编译环境经常会出问题。这是确实存在的，因为这些硬编码路径常常指向构建主机的位置。而它有可能是只读的，也有可能给交叉编译过程引入错误，这是由于他们是在构建主机上定制的而不是目标设备上。给Makefile打补丁添加变量的前缀或用其他变量替代来解决这类问题。
* 有时Makefile运行特定目标相关（target-specific ） 的命令如ldconfig。这些情况下，你需要对Makefile打补丁移除这些命令。

2.5.5. 添加本机工具
======
常常你需要编译在构建主机而不是目标设备上运行的辅助工具，当你用devtool add命令的时候可以用下面任意方法来实现这个需求：
* 指定配方的名字以“-native”结尾。这样指定名字后被构建的配方将会编译出主机程序。
* 运行devtool add命令的时候添加 “-also-native” 。指定这个选项后既可以创建构建设备软件的配方也可以创建带有“-native”变量来构建主机程序

2.5.6. 添加 Node.js 模块
======


2.6. 如何使用配方 Recipes
======

2.6.1. 查找日志和运行文件
======


2.6.2. 设置配置参数
======

2.6.3. 配方 Recipe 之间共享文件
======


2.6.4. 打包
======

2.7. 恢复目标设备的原始状态
======

2.8. 安装新增的条目到可扩展 SDK
======

2.9. 实现更新一个已安装的可扩展 SDK
======


2.10 创建一个带附加组件的衍生 SDK
======


via:https://docs.yoctoproject.org/sdk-manual/extensible.html



[1]: https://downloads.yoctoproject.org/releases/yocto/yocto-3.4.1/toolchain/
[2]: https://docs.yoctoproject.org/_images/sdk-devtool-add-flow.png
[3]: https://docs.yoctoproject.org/_images/sdk-devtool-modify-flow.png
[4]: https://docs.yoctoproject.org/ref-manual/variables.html#term-SRC_URI
[5]: https://docs.yoctoproject.org/dev-manual/common-tasks.html#upgrading-recipes
[6]: https://docs.yoctoproject.org/ref-manual/devtool-reference.html#devtool-the-workspace-layer-structure
[7]: https://docs.yoctoproject.org/bitbake/bitbake-user-manual/bitbake-user-manual-fetching.html#fetchers
[8]: https://docs.yoctoproject.org/_images/sdk-devtool-upgrade-flow.png
[9]: https://docs.yoctoproject.org/ref-manual/variables.html#term-DEPENDS
[10]: https://docs.yoctoproject.org/ref-manual/variables.html#term-LICENSE
[11]: https://docs.yoctoproject.org/ref-manual/variables.html#term-LIC_FILES_CHKSUM
[12]: https://docs.yoctoproject.org/ref-manual/variables.html#term-CC
[13]: https://docs.yoctoproject.org/ref-manual/variables.html#term-CXX
[14]: https://docs.yoctoproject.org/ref-manual/variables.html#term-EXTRA_OEMAKE
[15]: https://docs.yoctoproject.org/ref-manual/variables.html#term-PACKAGECONFIG_CONFARGS
