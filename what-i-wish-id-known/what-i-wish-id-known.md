文档概述 : 应知应会
======
__在进一步阅读之前，确认你已经给查阅了 [软件概述页面][1] ，其介绍了本文里很多相关的名词解释。同时下面其中有一些信息你现在不需要关心，但是当你着手开发的时候，这些信息会成为你的枕边书。这些都是 Yocto 项目开发的已知最适合的工作方法，我们会不定期的更新。__

————————-

__使用 Yocto 项目非常简单，但是出现问题就不好应付了。没有理解编译过程是如何工作的，你会发现你自己在尝试排查一个“黑盒子”的故障。这边有几条给新手开始第一次用 Yocto 项目编译之前他们希望已经知道的事项。如有其他建议请随时联系我们：__

1. __使用 Git，而不是 tar 包下载。__ 如果你用 git，软件根据 git 的运行情况将会自动更新修复问题。如果你下载 tar 包，你将需要自行更新。

2. __深入了解层索引__

所有的层可在 [层索引][2]找到。层主要应用于反映 Yocto 项目的兼容性状态（结构延续性的保证和科测试性），可以在[Yocto 项目的兼容性索引][3]中找到对应的层。一般首先需要检查兼容性索引，然后如果没有发现必要的层，去检查下通用层索引。层索引是来自嵌入式开源项目最基本的工件。因此，层索引没有专门的规划和测试而是 Yocto 项目直接提供的项目兼容性列表。但是后者有少量的条目。要知道你正在索引的层不是所有的层都有一样的成熟度，有效性或可用性。检索的时候也不会安装优先级显示。没有很简单的方法帮你挑选出最适合你的层。往往结果要反复试错，经过确认邮件列表，或和其他开发者一起协作才帮忙得到正确的选项。


3. __尽可能使用已知芯片的 BSP 层__

Intel, TI, NXP 和其他平台都有自己芯片对应的 BSP 层（layer）信息。这些层的名字都是类似于 “meta-intel” 或 “meta-ti”。不要尝试从头开始自己构建层。如果你需要对芯片定制，用现有的这些层作为指导书或模块，并熟悉 [Yocto 项目板级开发包（BSP）开发指南][4]。

4. __不要把所有的东西放到一个层__ 

使用不同的层在逻辑上分开编译信息。例如：你有一个 BSP 层，GUI 层，一个发行版配置，中间件，或一个应用程序（例如:"meta-filesystems","meta-python","meta-intel",等等）。把所有的编译到一个层将有很大的限制和很难对后续的定制和复用性进行兼容。在另一方面，将信息隔离多个层中帮助简化了后续定制和复用。

5. __永远不要修改 POKY 层__ 

永远不要，一旦你这么做了，当你升级基线到下一个版本，你会失去之前所有的工作。是的，所有工作。

6. __不要被搜索文档的结果所欺骗__
Yocto 项目文档会经常更新。不幸的是，当你用谷歌搜索 Yocto 项目概念或名词的时候，谷歌总是查找和检索 Yocto 项目的旧版本手册。比如：用谷歌在 Yocto 项目手册中搜索包含 “hit” 的指定标题的结果就是几个老版本的。确保你用最新的 Yocto 项目文档，最好在 [Yocto 项目文档页面][6]查找你软件版本对应的文档。如果你使用文档概述页面的搜索栏，这样搜索虽然不是最优的，但它会告诉你搜索的字符串在哪个文档。这样搜索会通常指出某个名词或概念的出处。


许多开发者通过检索 “Yocto 项目全文档集” 来查找 Yocto 项目手册的术语和名词。这个文档是对 Yocto 项目手册的核心集的整合。因此，用过 Ctrl-F 在这个手册搜索出所有 “hits” 的名词或术语。一旦你找到了你想要的地方，你就可以看到你想要的手册了。

7. 理解编译系统的如何工作的基本的概念：工作流
理解 Yocto 项目工作流是很重要，它帮你查明问题发生在哪里和编译时如何崩溃的。工作流分一下几个步骤：

1） Fetch - 获取源码
2） Extract - 解压源码
3） Patch - 为解决问题和新特性打补丁
4） Configure - 配置您的环境的一致性
5） Build - 编译和链接
6） Instal - 拷贝文件到目标目录
7） Package - 打包安装文件

在 “fetch” 过程中，可能无法获取到源码。在 “extract” 中，可能存在无效的压缩包或类似的问题。另一方面，工作流的特定的功能可能会让你了解出错在哪里。
![yp-how-it-works-new-diagram.png][5]

8. __需要知道的是你可以产生一个依赖图并学习如何用它__
A dependency graph shows dependencies between recipes, tasks, and targets. You can use the “-g” option with BitBake to generate this graph. When you start a build and the build breaks, you could see packages you have no clue about or have any idea why the build system has included them. The dependency graph can clarify that confustion. You can learn more about dependency graphs and how to generate them in the Generating Dependency Graphs section in the BitBake User Manual.

9. Here’s how you decode “magic” folder names in tmp/work
The build system fetches, unpacks, preprocesses, and builds. If something goes wrong, the build system reports to you directly the path to a folder where the temporary (build/tmp) files and packages reside resulting from the build. For a detailed example of this process, see the example. Unfortunately this example is on an earlier release of Yocto Project.

When you perform a build, you can use the “-u” BitBake command-line option to specify a user interface viewer into the dependency graph (e.g. knotty, ncurses, or taskexp) that helps you understand the build dependencies better.

10. You can build more than just images
You can build and run a specific task for a specific package (including devshell) or even a single recipe. When developers first start using the Yocto Project, the instructions found in the Yocto Project Quick Start show how to create an image and then run or flash that image. However, you can actually build just a single recipe. Thus, if some dependency or recipe isn’t working, you can just say “bitbake foo” where “foo” is the name for a specific recipe. As you become more advanced using the Yocto Project, and if builds are failing, it can be useful to make sure the fetch itself works as desired. Here are some valuable links: “Using a Development Shell” for information on how to build and run a specific task using devshell. Also, the SDK manual shows how to build out a specific recipe.

11. An ambiguous definition: Package vs Recipe
A recipe contains instructions the build system uses to create packages. Recipes and Packages are the difference between the front end and the result of the build process.

As mentioned, the build system takes the recipe and creates packages from the recipe’s instructions. The resulting packages are related to the one thing the recipe is building but are different parts (packages) of the build (i.e. the main package, the doc package, the debug symbols package, the separate utilities package, and so forth). The build system splits out the packages so that you don’t need to install the packages you don’t want or need, which is advantageous because you are building for small devices when developing for embedded and IoT.

12. You will want to learn about and know what’s packaged in rootfs

13. Create your own image recipe
There are a number of ways to create your own image recipe. We suggest you create your own image recipe as opposed to appending an existing recipe. It is trivial and easy to write an image recipe. Again, do not try appending to an existing image recipe. Create your own and do it right from the start.

14. Finally, here is a list of the basic skills you will need as a systems developer. You must be able to:

deal with corporate proxies
add a package to an image
understand the difference between a recipe and package
build a package by itself and why that’s useful
find out what packages are created by a recipe
find out what files are in a package
find out what files are in an image
add an ssh server to an image (enable transferring of files to target)
know the anatomy of a recipe
know how to create and use layers
find recipes (layers.openembedded.org)
understand difference between machine and distro settings
find and use the right BSP (machine) for your hardware
find examples of distro features and know where to set them
understanding the task pipeline and executing individual tasks
understand devtool and how it simplifies your workflow
improve build speeds with shared downloads and shared state cache
generate and understand a dependency graph
generate and understand bitbake environment
build an Extensible SDK for applications development
15.Depending on what you primary interests are with the Yocto Project, you could consider any of the following reading:

Look Through the Yocto Project Development Tasks Manual: This manual contains procedural information grouped to help you get set up, work with layers, customize images, write new recipes, work with libraries, and use QEMU. The information is task-based and spans the breadth of the Yocto Project.

Look Through the Yocto Project Application Development and the Extensible Software Development Kit (eSDK) manual: This manual describes how to use both the standard SDK and the extensible SDK, which are used primarily for application development. This manual also provides example workflows that use the popular Eclipse™ development environment and that use devtool. See the “Workflow using Eclipse™” and “Using devtool in your SDK Workflow” sections for more information.

Learn About Kernel Development: If you want to see how to work with the kernel and understand Yocto Linux kernels, see the Yocto Project Linux Kernel Development Manual. This manual provides information on how to patch the kernel, modify kernel recipes, and configure the kernel.

Learn About Board Support Packages (BSPs): If you want to learn about BSPs, see the Yocto Project Board Support Packages (BSP) Developer’s Guide. This manual also provides an example BSP creation workflow. See the “Developing a Board Support Package (BSP)” section.

Learn About Toaster: Toaster is a web interface to the Yocto Project’s OpenEmbedded build system. If you are interested in using this type of interface to create images, see the Toaster User Manual.

Have Available the Yocto Project Reference Manual: Unlike the rest of the Yocto Project manual set, this manual is comprised of material suited for reference rather than procedures. You can get build details, a closer look at how the pieces of the Yocto Project development environment work together, information on various technical details, guidance on migrating to a newer Yocto Project release, reference material on the directory structure, classes, and tasks. The Yocto Project Reference Manual also contains a fairly comprehensive glossary of variables used within the Yocto Project.
via：https://www.yoctoproject.org/docs/what-i-wish-id-known/


[1]: https://github.com/guevaraya/Yocto_doc/blob/master/software-overview/software-overview.md
[2]: http://layers.openembedded.org/
[3]: https://github.com/guevaraya/Yocto_doc/blob/master/software-overview/layer/index.md
[4]: https://github.com/guevaraya/Yocto_doc/blob/master/2.4/bsp-guide/bsp-guide.md
[5]: https://www.yoctoproject.org/wp-content/uploads/2017/07/yp-how-it-works-new-diagram.png
[6]: https://github.com/guevaraya/Yocto_doc/blob/master/3.0/bitbake-user-manual/bitbake-user-manual.md#generating-dependency-graphs
[7]: https://wiki.yoctoproject.org/wiki/Cookbook:Example:Adding_packages_to_your_OS_image
[8]:
[10]: 

