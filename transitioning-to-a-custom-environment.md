**[文档概要][1] : 过渡到定制环境** 

你已经看过[快速入门指南][2]也浏览了[应知应会][3]，后者包含了向他人学习的重要信息途径。虽然这样准备已经够了，但是当你着手自己的项目时就感到手足无措。鉴于有时文档让人望而却步，我们整理一些要点为你的着手提供帮助。

—————–
1. 列一个你工程的清单：处理器，目标板，相关技术和功能。你可以找到支持这些清单的菜谱(recipe)和其他元数据(meta)的层(layer)，然后将他们添加到你的配置里面。（参考第三条）


2.配置你的开发板。即便是你用定制的硬件，你也可以很容易的入手，因为都是已知的同类型处理器或至少是同样指令集的开发板。工程里已经包含的已知板级开发包让你更容易熟悉项目的基本概念。

3.为目标板找到最适合的板级开发包。使用 [Yocto 项目层维护的索引][4]或 [OpenEmbedded 的层索引][5]查找并获取最适合目标板的板级开发包，在Yocto 项目层索引里一般情况都能找得到。最好的是从你的半导体制造商或开发板厂商那里获取第一手的板级开发包-他们可以让其发挥最佳性能。通常来说，英特尔半导体使用 meta-intel，德州仪器使用 meta-ti 以此类推。最好选择一个在从Yocto 项目下载的并已经验证过的板级开发包。请注意一些板级开发包可能不会在最新的版本里立马支持，但是他们迟早还是会支持的。

你可能会使用 Poky 提供的编译手册（嵌入式参考发行版）并添加新的层到里面。下面是添加层的相关信息。

4.基于你选的层，做一些需求上的配置适配。例如，你选择一个设备类型添加到对应的板级开发包层（layer）。因此你需要修改配置文件（build/local.conf)里的 MACHINE 变量来指向相同类型的设备。这边也有其他需要指定的层配置。每个层都有自述文件（README）可以来查看它的使用说明。

5.为你创建的可定制的菜谱（recipes）和元数据（metadata）增加一个层（Layer）。使用 2.4 以上的 Yocto 发行版的 “bitbak-layer create-layer” 命令工具。如果你使用早于 2.4 的 Yocoto 发行版, 需要用 “yocto-layer create” 命令工具。“bitebak-layers” 命令工具也提供了其他有用的相关命令。查看[使用 bitbake-layer 命令创建一个普通层段落][7]

6.创建一个你自己将会使用的 BSP 层。这个和通常情况不一样，需要你从零开始创建一个整套 BSP 除非你已经有一个确定的真实设备。即使你正在使用已知的 BSP，也需要[创建一个你自己的 BSP 层][8]。例如，现有64位的X86机器，拷贝 conf/intel-corei7-64 的定义并命名一个机器名称（命名一个开发板名称，不是产品名）。确保层配置独立依赖 meta-intel 层（或至少 meta-intel 保留在你的 bblayers.conf)。现在你可以把您定制的 BSP 配置放到你的层里面，不同的应用程序可以重复使用它。 

7.编写你自己的菜谱（recipe）来构建还未支持菜谱（recipe）形式的软件。创建你自己的菜谱（recipe）对在设备里运行的自定义的应用程序的情况特别有用。编写新菜谱（recipe）是一个不断改进的过程。首先通过打包获取构建过程的步骤来入手，接着，在你的设备上运行程序然后再添枝加叶。查看在 Yocto 工程下[编写新菜谱（recipe）的开发任务手册][9]获取更多信息

8. 现在你要准备新建镜像的菜谱(recipe)了。这儿很多创建方式。但是强烈建议你创建自己的菜谱（recipes）-而不要在现有的镜像菜谱（recipes）上追加修改。新建镜像的菜谱（recipes）问题不大，主要是你常常需要深度定制他们的内容。

9. 构建你的镜像并完善它。根据你对[工作流程][10]的理解定位问题的出处，然后添加缺失的内容，修正受损的问题。

10. 根据情况创建自己的发行版。当你得到一定程度的定制，需要考虑创建自己的发行版比用原生参考发行版好一点。

发行版配置定义了打包后缀（如 rpm或其他类型），打包提要以及包的升级方案。创建自己的发行版，层（layer）要从 Poky 继承以覆盖要修改的配置。如果你在 local.conf 不仅添加了路径和其他典型配置之外还有大量的配置，那就该考虑[创建自己的发行版][11]了。

如果需要在其他层定制发行版，你可以添加其产品规格书。你也可以添加产品的其他特定功能。但要更新到发行版配置里，而不是单个产品里，你通过这种层更新发行版的特性。

恭喜你！你已经上手了。欢迎加入 Yocto 项目社区。

via: https://www.yoctoproject.org/docs/transitioning-to-a-custom-environment/

[1]: https://github.com/guevaraya/Yocto_doc
[2]: http://www.yoctoproject.org/docs/2.4/yocto-project-qs/yocto-project-qs.html
[3]: what-i-wish-id-known/what-i-wish-id-known.md
[4]: https://www.yoctoproject.org/software-overview/layers/
[5]: http://layers.openembedded.org/
[6]: http://www.yoctoproject.org/docs/current/dev-manual/dev-manual.html#understanding-and-creating-layersdocumentation
[7]: http://www.yoctoproject.org/docs/2.5/dev-manual/dev-manual.html#creating-a-general-layer-using-the-bitbake-layers-script
[8]: http://www.yoctoproject.org/docs/current/bsp-guide/bsp-guide.html#creating-a-new-bsp-layer-using-the-yocto-bsp-script
[9]: http://www.yoctoproject.org/docs/current/dev-manual/dev-manual.html#new-recipe-writing-a-new-recipe
[10]: http://www.yoctoproject.org/docs/current/sdk-manual/sdk-manual.html#using-devtool-in-your-sdk-workflow
[11]: http://www.yoctoproject.org/docs/2.5/dev-manual/dev-manual.html#creating-your-own-distribution

