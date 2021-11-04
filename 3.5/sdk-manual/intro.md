1.1.1. 交叉编译工具链
======

1.1.2. Sysroots
======

1.1.3. QEMU 模拟器
======

1.2. SDK 开发模型
======

1 介绍
====

1.1 eSDK 介绍
=================
欢迎阅读 Yocto 项目应用开发和可扩展软件套件的 eSDK 手册。本手册主要涉及如何使用 Yocto 可扩展套件和标准SDK 来开发应用程序和镜像的相关信息。

所有 SDK 的组成如下：
* **交叉工具链**：工具链包含一个编译器，调试器和许多相关工具。
* **库文件及其头文件，符号表**：指定镜像（例如 SDK编译的镜像匹配等）的库文件，头文件和符号表
* **环境变量配置脚本**：这个 *.sh* 脚本文件只需要运行一次，通过定义变量和做一些使用 SDK 的准备工作来配置交叉开发的环境变量。

另外，一个可扩展 SDK 里有工具允许你容易的添加一个应用程序和镜像的库文件，修改现有组件的源码，测试目标硬件的变化，并很容易的集成一个应用层序到 [OpenEmbedded 构建系统][4]里。

你可以用一个 SDK 去独立的开发和测试在一些目标机器运行的代码。这些 SDK 是完全独立的。二进制程序是链接到他们本地 libc 的拷贝，这样使其对目标系统没有依赖。为了实现这点，动态加载器的指针在安装的时候配置后地址就不能动态改变了。这就是为什么有封装 populate_sdk 和 populate_sdk_ext 的分别。

SDK 的另一个特性是用一套交叉工具链是构建任意指定的指令架构。这个特性的好处是目标硬件可以统一通过编译选项控制 gcc 编译。这些选项通过环境变量脚本配置生效并其包含在变量里，例如 [CC][5] 和 [LD][6]。这样会减少工具使用的内存占用。要知道，尽管是这样，每个目标设备仍然需要它的 sysroot 因为这些二进制是针对特定目标板的。

SDK 开发环境组成如下：

* 独立的 SDK 是指定架构的交叉编译和对应的 sysroot（目标文件和本地文件），这些都是通过 OpenEmbedded 构建系统编译处理的（例如 SDK）。交叉工具链和 sysroot 都是基于 Metadata 的配置和扩展的，这个允许在宿主机器上交叉开发目标硬件。另外，可扩展 SDK 包含了 devtool 的功能。
* QEMU 模拟器可以允许模拟目标硬件。它不是 SDK 里的一部分。你必须单独编译和引用这个模拟器。QENU 在围绕 SDK 程序开发扮演重要的角色。

总得来说，可扩展和标准的 SDK 有很多共同的特性。但是可扩展 SDK 作为强大的开发工具帮助你更快速的开发应用程序。下面的表格汇总了标准 SDK 和可扩展 SDK 之间在构建上的一些区别：

+-----------------------+-----------------------+-----------------------+
| *Feature*             | *Standard SDK*        | *Extensible SDK*      |
+=======================+=======================+=======================+
| Toolchain             | Yes                   | Yes [1]_              |
+-----------------------+-----------------------+-----------------------+
| Debugger              | Yes                   | Yes [1]_              |
+-----------------------+-----------------------+-----------------------+
| Size                  | 100+ MBytes           | 1+ GBytes (or 300+    |
|                       |                       | MBytes for minimal    |
|                       |                       | w/toolchain)          |
+-----------------------+-----------------------+-----------------------+
| ``devtool``           | No                    | Yes                   |
+-----------------------+-----------------------+-----------------------+
| Build Images          | No                    | Yes                   |
+-----------------------+-----------------------+-----------------------+
| Updateable            | No                    | Yes                   |
+-----------------------+-----------------------+-----------------------+
| Managed Sysroot [2]_  | No                    | Yes                   |
+-----------------------+-----------------------+-----------------------+
| Installed Packages    | No  [3]_              | Yes  [4]_             |
+-----------------------+-----------------------+-----------------------+
| Construction          | Packages              | Shared State          |
+-----------------------+-----------------------+-----------------------+

.. [1] Extensible SDK contains the toolchain and debugger if :term:`SDK_EXT_TYPE`
       is "full" or :term:`SDK_INCLUDE_TOOLCHAIN` is "1", which is the default.
.. [2] Sysroot is managed through the use of ``devtool``. Thus, it is less
       likely that you will corrupt your SDK sysroot when you try to add
       additional libraries.
.. [3] You can add runtime package management to the standard SDK but it is not
       supported by default.
.. [4] You must build and make the shared state available to extensible SDK
       users for "packages" you want to enable users to install.

The Cross-Development Toolchain
-------------------------------

The :term:`Cross-Development Toolchain` consists
of a cross-compiler, cross-linker, and cross-debugger that are used to
develop user-space applications for targeted hardware; in addition,
the extensible SDK comes with built-in ``devtool``
functionality. This toolchain is created by running a SDK installer
script or through a :term:`Build Directory` that is based on
your metadata configuration or extension for your targeted device. The
cross-toolchain works with a matching target sysroot.

Sysroots
--------

The native and target sysroots contain needed headers and libraries for
generating binaries that run on the target architecture. The target
sysroot is based on the target root filesystem image that is built by
the OpenEmbedded build system and uses the same metadata configuration
used to build the cross-toolchain.

The QEMU Emulator
-----------------

The QEMU emulator allows you to simulate your hardware while running
your application or image. QEMU is not part of the SDK but is
automatically installed and available if you have done any one of
the following:

-  cloned the ``poky`` Git repository to create a
   :term:`Source Directory` and sourced the environment setup script.

-  downloaded a Yocto Project release and unpacked it to
   create a Source Directory and sourced the environment setup
   script.

-  installed the cross-toolchain tarball and
   sourced the toolchain's setup environment script.

SDK Development Model
=====================

Fundamentally, the SDK fits into the development process as follows:

.. image:: figures/sdk-environment.png
   :align: center

The SDK is installed on any machine and can be used to develop applications,
images, and kernels. An SDK can even be used by a QA Engineer or Release
Engineer. The fundamental concept is that the machine that has the SDK
installed does not have to be associated with the machine that has the
Yocto Project installed. A developer can independently compile and test
an object on their machine and then, when the object is ready for
integration into an image, they can simply make it available to the
machine that has the Yocto Project. Once the object is available, the
image can be rebuilt using the Yocto Project to produce the modified
image.

You just need to follow these general steps:

1. *Install the SDK for your target hardware:* For information on how to
   install the SDK, see the ":ref:`sdk-manual/using:installing the sdk`"
   section.

2. *Download or Build the Target Image:* The Yocto Project supports
   several target architectures and has many pre-built kernel images and
   root filesystem images.

   If you are going to develop your application on hardware, go to the
   :yocto_dl:`machines </releases/yocto/yocto-&DISTRO;/machines/>` download area and choose a
   target machine area from which to download the kernel image and root
   filesystem. This download area could have several files in it that
   support development using actual hardware. For example, the area
   might contain ``.hddimg`` files that combine the kernel image with
   the filesystem, boot loaders, and so forth. Be sure to get the files
   you need for your particular development process.

   If you are going to develop your application and then run and test it
   using the QEMU emulator, go to the
   :yocto_dl:`machines/qemu </releases/yocto/yocto-&DISTRO;/machines/qemu>` download area. From this
   area, go down into the directory for your target architecture (e.g.
   ``qemux86_64`` for an Intel-based 64-bit architecture). Download the
   kernel, root filesystem, and any other files you need for your
   process.

   .. note::

      To use the root filesystem in QEMU, you need to extract it. See the
      ":ref:`sdk-manual/appendix-obtain:extracting the root filesystem`"
      section for information on how to do this extraction.

3. *Develop and Test your Application:* At this point, you have the
   tools to develop your application. If you need to separately install
   and use the QEMU emulator, you can go to `QEMU Home
   Page <https://wiki.qemu.org/Main_Page>`__ to download and learn about
   the emulator. See the ":doc:`/dev-manual/qemu`" chapter in the
   Yocto Project Development Tasks Manual for information on using QEMU
   within the Yocto Project.

The remainder of this manual describes how to use the extensible and
standard SDKs. There is also information in appendix form describing
how you can build, install, and modify an SDK.
