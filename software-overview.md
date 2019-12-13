入门指南: Yocto 项目® 概述
=======

![Yocto 软件概述][1]
Yocto 项目是一个开源协作项目，旨在帮助开发者创建一个类 Linux 系统的嵌入式产品，不用关心硬件架构信息。该项目提供一套灵活的工具和空间，全世界的嵌入式开发者可以分享技术，软件仓库，构建配置和为嵌入式设备创建的定制 Linux 镜像的优秀经验。

该项目提供了一套硬件适配和软件仓库的交付标准，允许软件的配置和构建可替代。这个工具允许用户可维护性和可伸缩性的构建和支持多个硬件平台和软件的定制化。

从历史经验看，大多项目都是从 [嵌入式开源项目][2] 共同成长起来的，其构建系统和元数据都是从嵌入式开源项目中衍生出来的。

Yocto 项目的组装，维护和验证是其三个关键的开发要素。

+ 1.  一套基础使嵌入式 Linux 出色的完成工作，包括自动构建和测试工具，单板适配和许可证遵循的处理，用于定制基于 嵌入式 Linux 操作系统的组件信息。
+ 2.  一个嵌入式参考发行版（名叫 Poky）
+ 3. 开源嵌入式构建系统与 [嵌入式开源项目][2] 一起维护。


在 Yocto 项目旗下 有很多不同的开源组件和工具。

Poky 是一个参考的嵌入式操作系统实例，它构建了一个包含编译系统的嵌入式小型操作系统（包含BiteBake，构建引擎和嵌入式开源核心，构建系统的核心元数据）。

构建系统和名叫 recipes 及 layers(下文有定义) 的 Poky 编译说明文件一同下载下来。为创建您的定制嵌入式 Linux，你可用任意方式拷贝或直接使用 Poky 构建细节。
![Yocto 构建][3]


层模板 - 定制化的关键
======

Yocto 项目拥有一个为嵌入式和物联网 Linux 构建的开发模板，这使它区别于其他简单的构建系统，这被称为层模板。

层模板的设计用来同时支持协作和定制开发。层库包含了告诉构建系统如何做的相关指令集。用户可协助，共享以及复用这些层。层可在任何时候包含对之前指令集和配置的修改。

强大的覆盖特性允许您对之前的协作或社区提供的层进行定制，以便满足您的产品需求。

使用不同的层逻辑上可在构建系统上隔离来。例如，你可以有一个板级支持包层，图形接口层，一个配置层，中间层，或一个应用层。如将您所有的构建限制到放一个层，后续的定制和复用有很大的复杂度。但是如果隔离出层来，另一方面来说简化未来的定制和复用。使用板级支持包层就使更换芯片厂商成为可能。

请自行熟悉正在筹划（测试中）的 [YOCTO 项目兼容性层的索引][4]。也有 [嵌入式开源层索引][5],它包含更多层，但是没有得到广泛的验证。

Yocto 项目成员组织可以提供关于特定板级支持包的信息。请访问 [Yocto 项目成员的板级支持包页面][6]获取详情。

Yocto 项目维护的组件和工具
======

![Yocto 组件][7]

Yocto 有一套组件和工具来维护和更新实际的项目。这些组件和工具也直接被 Yocto 项目自身使用。最后部分组件和工具被开发者用来创建他们的定制操作系统。这些组件和工具本身都是开源项目，开源元数据。他们都是独立于正式发行版和构建系统，他们大部分单独下载。

[更多关于组件和工具][7]

名称参考解释
=====
+ __配置文件（Configurations）：__ 这个文件管理全局变量的定义，用户变量的定义和硬件配置信息。它会告诉构建系统需要构建目标以及支持指定平台的镜像。
+ __菜谱（Recipes）：__ 最常见的元数据。一个菜谱将包含配置列表和任务（指令），它是用来构建二进制镜像的构建包。一个菜谱描述如何获取源码以及那些补丁需要合入。菜谱也描述了库和其他菜谱依赖关系，配置信息和兼容性选项。这些都保存在层中。


The most common form of metadata. A recipe will contain a list of settings and tasks (instructions) for building packages which are then used to build the binary image. A recipe describes where you get source code and which patches to apply. Recipes describe dependencies for libraries or for other recipes, as well as configuration and compilation options. They are stored in layers.

via:https://www.yoctoproject.org/software-overview/

[1]: https://www.yoctoproject.org/wp-content/uploads/2018/02/yp-diagram-overview.png
[2]: http://openembedded.org/
[3]: https://www.yoctoproject.org/wp-content/uploads/2018/02/yp-diagram-details.png
[4]: https://www.yoctoproject.org/software-overview/layers/
[5]: http://layers.openembedded.org/
[6]: https://www.yoctoproject.org/software-overview/layers/bsps/
[7]: https://www.yoctoproject.org/wp-content/uploads/2018/02/yp-diagram-yocto.png
[8]: https://www.yoctoproject.org/software-overview/project-components/

