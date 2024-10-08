
### 技术概述
<!-- GFM-TOC -->
* [1. 入门指南](#getting_started)
* [2. Layer模型](#layer-model)
* [3. 组件和工具](#components-tools)
* [4. 相关术语](#terms-of-reference)
* [5. 常规流程](#general-workflow)
* [6. 开发环境](#dev-environment)
* [7. 嵌入式参考发行版（Poky）](#poky)
* [8. Yocto特性](#features)

<a id="getting_started">入门指南: Yocto 项目® 概述</a>
=======

![Yocto 软件概述][1]
Yocto 项目是一个开源协作项目，旨在帮助开发者创建一个类 Linux 系统的嵌入式产品，不用关心硬件架构信息。该项目提供一套灵活的工具和空间，全世界的嵌入式开发者可以共享科技，软件技术，构建配置已经创建为定制 Linux 和嵌入式软件镜像的优秀经验。

该项目提供了一套硬件适配和软件仓库的交付标准，有利于软件的配置和构建的交流互通。这让硬件提供商和使用者围绕软件生态良性互动。这个工具也允许用户可维护性和可伸缩性的构建和支持多个硬件平台和软件的定制化。

从历史经验看，大多项目都是从 [嵌入式开源项目][2] 共同成长起来的，其构建系统和元数据都是从嵌入式开源项目中衍生出来的。

Yocto 项目的组装，维护有几个关键的开发要素。

1. OpenEmbedded 构建系统 与OpenEmbedded-Core 和 BitBake组成的 OpenEmbedded项目共同维护运营 
1. 用于测试的参考/示例嵌入式Linux配置（名叫 Poky）
1. 通过我们的自动编译Buildbot提供广泛的测试基础设施
1. 使嵌入式Linux流畅运行的集成工具
    * 自动编译和测试工具
    * 板级支持定义和通信的标准和处理
    * 相关安全工具和许可证兼容，在SPDX的支持软件清单
 


在 Yocto 项目旗下 有很多不同的开源组件和工具。

Poky 是一个参考的嵌入式操作系统实例，它构建了一个包含编译系统的嵌入式小型操作系统（包含BiteBake，构建引擎和嵌入式开源核心，构建系统的核心元数据）。

构建系统和名叫 recipes 及 layers(下文有定义) 的 Poky 编译说明文件一同下载下来。为创建您的定制嵌入式 Linux，你可用任意方式拷贝或直接使用 Poky 构建细节。
![Yocto 构建][3]


<a id="layer-model">层模型 - 定制化的关键</a>
======

Yocto 项目拥有一个为嵌入式和物联网 Linux 构建的开发模型，这使它区别于其他简单的构建系统，这被称为层模板。

层模板的设计用来支持同时协作和定制开发。层库就是包含让构建系统如何做的相关指令集的代码仓。用户可协助，共享以及复用这些层。层包含对之前任意时间的指令集和配置的修改。

强大的覆盖特性允许您对之前的协作或社区提供的层进行定制，以便满足您的产品需求。

使用不同的层逻辑上可在构建系统上隔离来。例如，你可以有一个板级支持包层，图形接口层，一个配置层，中间层，或一个应用层。如将您所有的构建限制到放一个层，后续的定制和复用有很大的复杂度。但是如果隔离出层来，另一方面来说简化未来的定制和复用。使用板级支持包层就使更换芯片厂商成为可能。

请自行熟悉正在筹划（测试中）的 [YOCTO 项目兼容性层的索引][4]。也有 [嵌入式开源层索引][5],它包含更多层，但是没有得到广泛的验证。


<a id="components-tools">Yocto 项目维护的组件和工具</a>
======

Yocto 有一套组件和工具来维护和更新实际的项目。这些组件和工具也直接被 Yocto 项目自身使用。最后部分组件和工具被开发者用来创建他们的定制操作系统。这些组件和工具本身都是开源项目，开源元数据。他们都是独立于正式发行版和构建系统，他们大部分可单独下载。

[更多关于组件和工具][7]

<a id="terms-of-referenc">相关术语</a>
=====
+ __配置文件（Configurations）：__ 这个文件保存全局变量的定义，用户变量的定义和硬件配置信息。它会告诉构建系统如何构建以及适配指定平台的镜像。
+ __配方（Recipes）：__ 最常见的元数据。一个配方将包含配置列表和任务（指令），它是用来构建二进制镜像的构建包。一个配方描述了如何获取源码以及需要打那些补丁。配方也描述了库和其他配方依赖关系，还有配置信息和兼容性选项。这些配置文件都保存在层中。
+ __层（Layer）：__ 集合了相关的配方。层允许您为了定制构建来整合相关的元数据，同时隔离多个架构的构建。层是分层的并可以覆盖之前的规则。您可囊括 Yocto 项目的任意个数的有效层并将它们添加定制到您自己的层。层的索引在 Yocto 项目中可易于检索。
+ __元数据（Metadata）：__ Yocto 的一个主要元数据是 Meta-data，它是用来构造一个 Linux 发行版，其包含了构建镜像时的编译系统所用的解析文件。一般情况下，Metadata 包含 Recipe，配置文件和其他构建指令自身的相关信息，也有用于控制构建目标和如何构建的数据。Meta-data 也包含用于指示所用软件版本的命令和数据，以及从哪儿获取，还有软件自己的修改和新增（补丁和辅助额外文件）信息，这些文件用来修正问题或为了特定需求定制软件的。OpenEmbedded 核心是一套重要的被验证过的 metadata。
+ __泡壳（Poky）：__ 一个嵌入式参考发行版和一个测试性的参考配置被创建出来：
  1) 提供基础功能的发行版，它可以用来说明如何定制一个发行版；
  2) 用来测试 Yocto 项目的组件，Poky 用来验证 Yocto 项目
  3) 一个为用户下载 Yocto 项目的传播媒介。泡壳本身不是一个产品界别的发行版，但是一个项目定制的不错的起点。泡壳是 oe-core 上的集成层。

+ __构建系统-“Bitbake”：__ 是一个调度器和执行引擎，它可分析 recipe 的指令和配置数据。然后为编译创建一个依赖树，调度包含代码的编译，最后执行定制 Linux 镜像（发行版）的构建。Bitbake 是一个类 make 的编译工具。Bitbake 的配方指定了一个特定包如何编译。他们包含包之间的依赖关系，源码位置，配置信息，编译信息，构建，安装和移除指令。配方也为包保存变量里的 Metadata。相关的配方被整合到一个层里。在构建过程中跟踪依赖关系来执行本地和交叉包编译。交叉编译的第一个步，编译框架将会为目标平台尝试创建一个交叉编译器（可扩展的 SDK）。
+ __程序包（packages）：__ 构建系统的输出用来创建您的最终镜像。
+ __可扩展软件开发包（ESDK）：__ 为开发者提供一个定制的 SDK ，允许他们的库文件和编程的改动整合到镜像里，这样可供其他的应用开发使用。
+ __镜像（Image）：__ 一个被装载到一个设备上的Linux 发行版（操作系统）镜像。


<a id="terms-of-reference">常规流程- 他是如何运行的</a>
====== 

![流程图][9]
+ 首先，开发者指定体系架构，开源协议，补丁和配置等信息。
+ 编译系统然后从指定的地方获取和下载源码。我们项目支持标准的 tar 包或 Git 仓的方式下载源码。
+ 一旦下载了源码，将其解压到本地工作区域，打上补丁同时运行常用的步骤：配置和编译。
+ 软件然后会被安装到一个暂存区域，在这里打包生成你指定的二进制包整合成指定格式（deb，rpm 或 ipk的）软件镜像。
+ 整个编译过程会进行二进制包的质量和完整性检查。
+ 当创建二进制文件后，生成一个二进制包摘要，然后用它来创建根文件镜像。
+ 最后生成文件系统镜像。

当使用 Yocto 工程的时候，这个流程根据你实际使用的组件和工具会有变化。

<a id="dev-environment">开发环境</a>
======
大部分开发者使用Linux主机作为开发环境，详见[快速构建Yocto 项目][6]


<a id="poky">嵌入式参考发行版（Poky）</a>
======
“Poky” 是 Yocto 项目的参考发行版或参考系统集的简写。它包含了编译系统（Bitbake和嵌入式开源核心组件）和一套用来编译发行版的数据集。

如果想使用 Yocto 工程工具，你可下载 Poky 来定制您的发行版。请注意 Poky 不包含二进制文件-它仅是一个如何从源码构建您的 Linux 发行版的示例。

[更多关于参考发行版][11]

<a id="features">Yocto 项目特性</a>
======

除了固有的特性外，Yocto 项目有一些特性在一个或多个发布版本中扩展和体现。更进一步的功能特性在 “readme” 文件与正式版本和工具一同发布。它提供了高级的特性概述，包含最近三个版本的特性和通用特性。

[高级特性列表][12]

via:https://www.yoctoproject.org/software-overview/

译者：[巴龙](https://github.com/guevaraya)
校对：[校对者ID](https://github.com/校对者ID)
版权所有© Linux 基金会

[1]: https://www.yoctoproject.org/wp-content/uploads/2018/02/yp-diagram-overview.png
[2]: http://openembedded.org/
[3]: https://www.yoctoproject.org/wp-content/uploads/sites/32/2023/10/Yocto-Tech-Overview-Graphic.svg
[4]: https://www.yoctoproject.org/software-overview/layers/
[5]: http://layers.openembedded.org/
[6]: https://docs.yoctoproject.org/brief-yoctoprojectqs/index.html
[7]: https://docs.yoctoproject.org/overview-manual/yp-intro.html#components-and-tools
[8]: https://www.yoctoproject.org/software-overview/project-components/
[9]: https://www.yoctoproject.org/wp-content/uploads/2017/07/yp-how-it-works-new-diagram.png
[10]: https://www.yoctoproject.org/wp-content/uploads/2018/02/os-logos.png
[11]: https://docs.yoctoproject.org/ref-manual/terms.html#term-Poky
[12]: https://docs.yoctoproject.org/overview-manual/yp-intro.html#features

