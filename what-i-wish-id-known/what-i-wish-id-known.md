文档概述 : 应知应会
======
__在进一步阅读之前，确认你已经给查阅了 [软件概述页面][1] ，其介绍了本文里很多相关的名词解释。同时下面其中有一些信息你现在不需要关心，但是当你着手开发的时候，这些信息会成为你的枕边书。这些都是 Yocto 项目开发的已知最适合的工作方法，我们会不定期的更新。__

————————-

__使用 Yocto 项目非常简单，但是出现问题就不好应付了。没有理解编译过程是如何工作的，你会发现你自己在尝试排查一个“黑盒子”的故障。这边有几条给新手开始第一次用 Yocto 项目编译之前他们希望已经知道的事项。如有其他建议请随时联系我们：__

1. __使用 Git，而不是 tar 包下载。__ 如果你用 git，软件根据 git 的运行情况将会自动更新修复问题。如果你下载 tar 包，你将需要自行更新。

2. Get to know the layer index
All layers can be found in the layer index. Layers which have applied for Yocto Project Compatible status (structure continuity assurance and testing) can be found in the Yocto Project Compatible index. Generally check the Compatible layer index first, and if you don’t find the necessary layer check the general layer index. The layer index is an original artifact from the Open Embedded Project. As such, that index doesn’t have the curating and testing that the Yocto Project provides on Yocto Project Compatible layer list, but the latter has fewer entries. Know that when you start searching in the layer index that not all layers have the same level of maturity, validation, or usability. Nor do searches prioritize displayed results. There is no easy way to help you through the process of choosing the best layer to suit your needs. Consequently, it is often trial and error, checking the mailing lists, or working with other developers through collaboration rooms that can help you make good choices.

3. Use existing BSP layers from silicon vendors when possible
Intel, TI, NXP and others have information on what BSP layers to use with their silicon. These layers have names such as “meta-intel” or “meta-ti”. Try not to build layers from scratch. If you do have custom silicon, use one of these layers as a guide or template and familiarize yourself with the Yocto Project Board Support Package (BSP) Developer’s Guide.

4. Do not put everything into one layer
Use different layers to logically separate information in your build. As an example, you could have a BSP layer, a GUI layer, a distro configuration, middleware, or an application (e.g. “meta-filesystems”, “meta-python”, “meta-intel”, and so forth). Putting your entire build into one layer limits and complicates future customization and reuse. Isolating information into layers, on the other hand, helps keep simplify future customizations and reuse.

5. Never modify the POKY layer. Never. Ever. When you update to the next release, you’ll lose all of your work. ALL OF IT.

6. Don’t be fooled by documentation searching results
Yocto Project documentation is always being updated. Unfortunately, when you use Google to search for Yocto Project concepts or terms, Google consistently searches and retrieves older versions of Yocto Project manuals. For example, searching for a particular topic using Google could result in a “hit” on a Yocto Project manual that is several releases old. To be sure that you are using the most current Yocto Project documentation, use the Yocto Project documentation page to locate the right documentation for your software release version. If you use the search bar on the top of the Documentation Overview page, while that search isn’t optimal, it will point you to the documents where your search string can be found. That search will usually identify where most of the attention on a given term or concept is.

Many developers look through the complete Yocto Project set of manuals for a concept or term by doing a search through the “Yocto Project Complete Documentation Set”. This manual is a concatenation of the core set of Yocto Project manual. Thus, a simple string search using Ctrl-F in this manual produces all the “hits” for a desired term or concept. Once you find the area in which you are interested, you can display the actual manual, if desired.

7. Understand the basic concepts of how the build system works: the workflow
Understanding the Yocto Project workflow is important as it can help you both pinpoint where trouble is occurring and how the build is breaking. The workflow breaks down into the following steps:

1) Fetch – get the sourcecode,
2) Extract – unpack the sources
3) Patch – apply patches for bug fixes and new capability
4) Configure – set up your environment specifications
5) Build – compile and link
6) Install – copy files to target directories
7) Package – bundle files for installation.
During “fetch”, there may be an inability to find code. During “extract”, there is likely an invalid zip or something similar. In other words, the function of a particular part of the workflow gives you an idea of what might be going wrong.



8. Know that you can generate a dependency graph and learn how to do it
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
[6]: http://www.yoctoproject.org/docs/current/bitbake-user-manual/bitbake-user-manual.html#generating-dependency-graphs
[7]: https://wiki.yoctoproject.org/wiki/Cookbook:Example:Adding_packages_to_your_OS_image
[8]:
[10]: 

