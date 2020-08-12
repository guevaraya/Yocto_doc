**[文档概要][1] : 过渡到定制环境** 

你已经看过[快速入门指南][2]并浏览了[应知应会][3]，后者包含了向其他用户学习的重要信息。虽然这样准备可以了，但是当你着手你自己的项目的时候就感到手足无措。鉴于有时文档让人望而却步，我们整理一些提示为你的着手提供帮助。

—————–
1. Make a list of the processor, target board, technologies, and capabilities that will be part of your project. You will be finding layers with recipes and other metadata that support these things, and adding them to your configuration. (See #3)
1. 列一个你工程的清单：处理器，目标板，相关技术和功能。你可以找到支持这些清单的菜谱和其他元数据的层，然后将他们增加到你的配置里面。（参考第三条）


2. Set up your board support. Even if you’re using custom hardware, it might be easier to start with an existing target board that uses the same processor or at least the same architecture as your custom hardware. Knowing the board already has a functioning Board Support Package (BSP) within the project makes it easier for you to get comfortable with project concepts.
2.配置你的开发板。即便是你用定制的硬件，你也可以很容易的入手已知的同类型处理器或至少是同样指令集的开发板。工程里已经包含的已知板级开发包让你更容易熟悉项目的概念。

3. Find and acquire the best BSP for your target. Use the Yocto Project® curated layer index or even the OpenEmbedded layer index to find and acquire the best BSP for your target board. The Yocto Project layer index BSPs are regularly validated. The best place to get your first BSP is from your silicon manufacturer or board vendor – they can point you to their most qualified efforts. In general, for Intel silicon use meta-intel, for Texas Instruments use meta-ti, and so forth. Choose a BSP that has been tested with the same Yocto Project release that you’ve downloaded. Be aware that some BSPs may not be immediately supported on the very latest release, but they will be eventually.
3.为目标板找到最适合的板级开发包.使用 [Yocto 项目层维护的索引][4]或 [OpenEmbedded 的层索引][5]查找并获取最适合你的目标板的板级开发包。在Yocto 项目层索引里一般情况都能找得到。最好的是从你的半导体制造商或开发板厂商哪里获取第一手的板级开发包-他们可以告诉你他们的最佳性能。通常来说，英特尔半导体使用 meta-intel，德州仪器使用 meta-ti 以此类推。选择一个在 你从Yocto 项目下载的并已经验证过的板级开发包。请注意一些板级开发包可能不会在最新的版本里立马支持，但是他们最终还是会支持的。

You might want to start with the build specification that Poky provides (which is reference embedded distribution) and then add your newly chosen layers to that. Here is the information [about adding layers][6].
你可能会使用 Poky 提供的编译手册（嵌入式参考发行版）并增加新的层到里面。下面是增加层的相关信息。

4. Based on the layers you’ve chosen, make needed changes in your configuration. For instance, you’ve chosen a machine type and added in the corresponding BSP layer. You’ll then need to change the value of the MACHINE variable in your configuration file (build/local.conf) to point to that same machine type. There could be other layer-specific settings you need to change as well. Each layer has a README document that you can look at for this type of usage information.
4.基于你选的层，做一些需求上的配置修改。例如，你选择一个设备类型到对应的 板级开发包。然后你需要修改配置文件（build/local.conf)里的 MACHINE 变量来指向相同类型的设备。这边也有其他需要指定的层配置。每个层都有自述文件（README）可以来查看它的使用说明。

5. Add a new layer for any custom recipes and metadata you create. Use the “bitbake-layers create-layer” tool for Yocto Project 2.4+ releases. If you are using a Yocto Project release earlier than 2.4, use the “yocto-layer create” tool. The “bitbake-layers” tool also provides a number of other useful layer-related commands. Creating a general layer using the bitbake-layers Command” section.
5.为你创建的可定制的菜谱（recipes）和元数据（metadata）增加一个层（Layer）。使用 2.4 以上的Yocoto 发行版的 “bitbak-layer create-layer” 命令工具。如果你使用早于 2.4 的 Yocoto 发行版, 需要用 “yocto-layer create” 命令工具。“bitebak-layers” 命令工具也提供了其他有用的相关命令。[使用 bitbake-layer 命令创建一个普通层段落][7]

6. Create your own layer for the BSP you’re going to use. It is not common that you would need to create an entire BSP from scratch unless you have a *really* special device. Even if you are using an existing BSP, create your own layer for the BSP. For example, given a 64-bit x86-based machine, copy the conf/intel-corei7-64 definition and give it a machine a relevant name (think board name, not product name). Make sure the layer configuration is dependent on the meta-intel layer (or at least, meta-intel remains in your bblayers.conf). Now you can put your custom BSP settings into your layer and you can re-use it for different applications.
6.创建一个你自己将会使用的 BSP 层。这个和通常情况不一样，需要你从零开始创建一个整套 BSP 除非你已经有一个确定的真实设备。即使你正在使用已知的 BSP，也需要[创建一个你自己的 BSP 层][8]。例如，现有64位的X86机器，拷贝 conf/intel-corei7-64 的定义并命名一个机器名称（命名一个开发板名称，不是产品名）。确保层配置独立依赖 meta-intel 层（或至少 meta-intel 保留在你的 bblayers.conf)。现在你可以把您定制的 BSP 配置放到你的层里面，不同的应用程序可以重复使用它。 

7. Write your own recipe to build additional software support that isn’t already available in the form of a recipe. Creating your own recipe is especially important for custom application software that you want to run on your device. Writing new recipes is a process of refinement. Start by getting each step of the build process working beginning with fetching all the way through packaging. Next, run the software on your target and refine further as needed. See Writing a New Recipe in the Yocto Project Development Tasks Manual for more information.
7.编写你自己的菜谱（recipe）来构建还未支持菜谱（recipe）形式的的的软件。创建你自己的菜谱（recipe）对在设备里运行的自定义的应用程序的情况下特别有用。编写新菜谱（recipe）是一个不断改进的过程。首先通过打包获取构建过程的步骤来入手，接着，在你的设备上运行程序然后再添枝加叶。查看在 Yocto 工程下[编写新菜谱（recipe）的开发任务手册][9]获取更多信息

8. Now you’re ready to create an image recipe. There are a number of ways to do this. However, it is strongly recommended that you have your own image recipe – don’t try appending to existing image recipes. Recipes for images are trivial to create andf you usually want to fully customize their contents.
8. 现在你要准备新建镜像的菜谱(recipe)了。这儿很多创建方式。但是强烈建议你创建自己的菜谱（recipes）-而不要在现有的镜像菜谱（recipes）上追加修改。新建镜像的菜谱（recipes）问题不大，主要是你常常需要深度定制他们的内容。

9. Build your image and refine it. Add what’s missing and fix anything that’s broken using your knowledge of the workflow to identify where issues might be occurring.
9. 构建你的镜像并完善它。用你对工作流程的知识定位问题问题的出处，然后添加缺失的内容，修正受损的问题。

10. Consider creating your own distribution. When you get to a certain level of customization, consider creating your own distribution rather than using the default reference distribution.
10. 伺机创建自己的发行版。当你得到一定程度的定制，需要考虑创建自己的发行版比用原生参考发行版好一点。

Distribution settings define the packaging back-end (e.g. rpm or other) as well as the package feed and possibly the update solution. You would create your own distribution in a new layer inheriting from Poky but overriding what needs to change for your distribution. If you find yourself adding a lot of configuration to your local.conf file aside from paths and other typical local settings, it’s time to consider creating your own distribution.
发行版配置定义了打包后缀（如 rpm或其他类型），打包提要以及包的升级方案。创建自己的发行版，层（layer）要从 Poky 继承以覆盖要修改的配置。如果你在 local.conf 不仅添加了路径和其他典型配置之外还有大量的配置，那就该考虑创建自己的发行版了。

You can add product specifications that can customize the distribution if needed in other layers. You can also add other functionality specific to the product. But to update the distribution, not individual products, you update the distribution feature through that layer.

11. Congratulations! You’re well on your way. Welcome to the Yocto Project community.

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

