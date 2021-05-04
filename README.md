<!-- omit in toc -->
# BesLyric-for-X Development Environment Configuration (Windows MSVC x64, Chinese only)

<!-- omit in toc -->
## 简介

本仓库中的文档用于 Windows MSVC x64 上的 BesLyric-for-X 的开发环境配置。

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议</a>进行许可。

<!-- omit in toc -->
## 导航

返回[主分支](../main/README.md)。

<!-- omit in toc -->
## 注意

- 本文档可能不提供通过编译源码进行环境配置的方法；
- 本文档可能不提供有关已结束支持的操作系统 <sup><a href="#note_1" id="note_1_sup">[1]</a></sup> 和软件 <sup><a href="#note_2" id="note_2_sup">[2]</a></sup> <sup><a href="#note_3" id="note_3_sup">[3]</a></sup> 的内容；
- BesLyric-for-X 仅支持 Qt 5.12.4 及更高版本；
- BesLyric-for-X 仅支持 MSVC 14.1 及更高版本；
- BesLyric-for-X 仅支持 Windows SDK 10.0.17763.0 及更高版本。

<!-- omit in toc -->
## 目录

- [1. 有效性参考](#1-有效性参考)
- [2. BesLyric-for-X 项目配置](#2-beslyric-for-x-项目配置)
  - [2.1. 环境变量`B4X_DEP_PATH`](#21-环境变量b4x_dep_path)
  - [2.2. 构建目标](#22-构建目标)
- [3. 配置开发环境](#3-配置开发环境)
  - [3.1. C++ 开发环境](#31-c-开发环境)
    - [3.1.1. MSVC 工具集 & Windows SDK](#311-msvc-工具集--windows-sdk)
    - [3.1.2. CDB 调试器](#312-cdb-调试器)
  - [3.2. Qt 5.12.4+](#32-qt-5124)
    - [3.2.1. 安装步骤](#321-安装步骤)
    - [3.2.2. 注意事项](#322-注意事项)
  - [3.3. 第三方库](#33-第三方库)
    - [3.3.1. SDL 2](#331-sdl-2)
    - [3.3.2. FFmpeg 4](#332-ffmpeg-4)
    - [3.3.3. OpenSSL 1.1.1](#333-openssl-111)
- [4. 脚注](#4-脚注)
- [5. 参考](#5-参考)
- [6. 其他](#6-其他)

## 1. 有效性参考

按照本文档进行配置后，通过测试的操作系统与开发环境的版本组合：

| OS &<br>ReleaseID <sup><a href="#note_4" id="note_4_sup">[4]</a></sup> &<br>Kernel <sup><a href="#note_5" id="note_5_sup">[5]</a></sup> | %VCToolsVersion% | Windows SDK | Qt | SDL | FFmpeg | TLS Impl. |
|:- |:- |:- |:- |:- |:- |:- |
| <table><tr><td>Microsoft Windows 10 Pro &amp;<br>2004 &amp;<br>Microsoft Windows [Version 10.0.19041.329]</td></tr></table> | 14.26.28801 | 10.0.18362.1 | 5.15.0 | 2.0.12 | ffmpeg version 4.3.1 built with gcc 10.2.1 (GCC) 20200726 | OpenSSL 1.1.1g  21 Apr 2020 |
| <table><tr><td>Microsoft Windows 10 Pro &amp;<br>2004 &amp;<br>Microsoft Windows [Version 10.0.19041.329]</td></tr></table> | 14.16.27023 | 10.0.17763.0 | 5.12.9 | 2.0.12 | ffmpeg version 4.3.1 built with gcc 10.2.1 (GCC) 20200726 | OpenSSL 1.1.1g  21 Apr 2020 |
| <table><tr><td>Microsoft Windows 10 Pro &amp;<br>1909 &amp;<br>Microsoft Windows [Version 10.0.18363.592]</td></tr><tr><td>Microsoft Windows 10 Pro &amp;<br>1903 &amp;<br>Microsoft Windows [Version 10.0.18362.592]</td></tr><tr><td>Microsoft Windows 10 Enterprise LTSC &amp;<br>1809 &amp;<br>Microsoft Windows [Version 10.0.17763.973] <sup><a href="#note_6" id="note_6_sup">[6]</a></sup> </td></tr></table> | 14.24.28314 | 10.0.18362.1 | 5.14.1 | 2.0.10 | ffmpeg version 4.2.2 built with gcc 9.2.1 (GCC) 20200122 | OpenSSL 1.1.1d  10 Sep 2019 |

## 2. BesLyric-for-X 项目配置

### 2.1. 环境变量`B4X_DEP_PATH`

为了正确配置 BesLyric-for-X 所需的第三方库，在 Windows MSVC 平台，需要设置环境变量`B4X_DEP_PATH`。

`B4X_DEP_PATH`的值应该仅包含操作系统允许在路径中使用的 ASCII 字符，同时，不应该包含空格，两端也不需要添加引号，例如`C:\b4x-lib`。

`B4X_DEP_PATH`指向的文件夹应该按照规定的结构包含所有需要的文件（头文件、静态链接库文件和动态链接库文件）。 BesLyric-for-X 中的 qmake 指令将分别向变量`$$INCLUDEPATH`和`$$LIB`添加`%B4X_DEP_PATH%\include`和`-L%B4X_DEP_PATH%\lib`，使 Qt 和 MSVC 能够找到第三方库的头文件和静态链接库文件。

不建议将该环境变量配置到操作系统或当前用户中，而是应该配置到项目内。例如，通过 Qt Creator 的左侧列表切换到“ Projects ”页面，在“ Build & Run ”-“ Build Settings ”-“ Build Environment ”或“ Project Settings ”-“ Environment ”中配置环境变量 <sup><a href="#note_7" id="note_7_sup">[7]</a></sup> 。

### 2.2. 构建目标

有 5 个构建目标可供使用：

- `deploy_3rd_party_library_dlls`
- `windeployqt_enhanced`
- `deploy_CRT_dlls`
- `deploy_UCRT_dlls`
- `deploy_windows_dlls`

其中，`deploy_windows_dlls`目标依赖于其他所有的目标，且上述所有目标均依赖于`first`目标。

## 3. 配置开发环境

### 3.1. C++ 开发环境

#### 3.1.1. MSVC 工具集 & Windows SDK

需要使用 [Visual Studio Installer](https://visualstudio.microsoft.com/) 安装“ Desktop development with C++ ”工作负载以安装 MSVC 工具集和 Windows SDK 。

MSVC 工具集包含 C/C++ 编译器（`cl.exe`）、资源编译器（`rc.exe`）、链接器（`link.exe`）、构建工具（`nmake.exe`）等程序。

Windows SDK 包含 Windows 开发所需要的文档、头文件（例如`Windows.h`和`stdio.h`）、库文件（例如`ncrypt.lib`和`libucrt.lib`）、调试器（例如 CDB ）等组件。

#### 3.1.2. CDB 调试器

CDB 与 WinDbg 同为 Windows 上的调试器 <sup><a href="#note_8" id="note_8_sup">[8]</a></sup> ， Qt Creator 需要使用 CDB 调试程序。

通过 Visual Studio Installer 安装 Windows SDK 后，在“应用和功能”列表中找到“ Windows Software Development Kit - Windows 10... ”，选择它并单击“修改”，弹出“ Windows Software Development Kit - Windows 10... ”窗口。在“ Maintain your Windows Software Development Kit - Windows 10... features ”页面选择“ Change ”，下一步后，在“ Select the features you want to change ”页面勾选“ Debugging Tools for Windows ”并单击“ Change ”以安装 CDB <sup><a href="#note_9" id="note_9_sup">[9]</a></sup> 。

### 3.2. Qt 5.12.4+

#### 3.2.1. 安装步骤

1. 从 [Index of /official_releases/online_installers](http://download.qt.io/official_releases/online_installers/) 下载在线安装程序；
2. 勾选当前开发环境对应的预构建组件（ Pre-built Components ），例如“ Qt - Qt 5.15.2 - MSVC 2019 64-bit ”；
3. 勾选调试器的支持组件，例如“ Qt - Developer and Designer Tools - Qt Creator 4.14.0 CDB Debugger Support ”；如果之前已经通过 Windows SDK 安装了 CDB ，就**取消勾选**默认随 Qt 一起安装的调试工具，例如“ Qt - Developer and Designer Tools - Debugging Tools for Windows ”，以避免重复的安装。

#### 3.2.2. 注意事项

- MSVC 2015 、 2017 和 2019 二进制兼容 <sup><a href="#note_10" id="note_10_sup">[10]</a></sup> ，所以“ MSVC 2015 ”和“ MSVC 2017 ”版本的 Qt 组件都可用于 MSVC 2019 的开发环境 <sup><a href="#note_11" id="note_11_sup">[11]</a></sup> ；
- 如果 Qt Creator 在 General Messages 中输出`C and C++ compiler paths differ. C compiler may not work.`，就需要检查各个 Kit （构建套件）中的 C 和 C++ 编译器的路径是否一致。

### 3.3. 第三方库

#### 3.3.1. SDL 2

1. 下载用于 Windows 的、 devel 版本的、用于 Visual C++ 的 [SDL 2](https://www.libsdl.org/download-2.0.php) ；
2. 将各个文件从压缩包中按照类型提取到指定位置：
   1. 提取`\SDL2-...\include\`下的所有`.h`文件到`%B4X_DEP_PATH%\include\SDL2\`下；
   2. 提取`\SDL2-...\lib\x64\`下的`SDL2.lib`和`SDL2main.lib`文件到`%B4X_DEP_PATH%\lib\`下；
   3. 提取`\SDL2-...\lib\x64\`下的`SDL2.dll`文件到`%B4X_DEP_PATH%\bin\`下。

<details open>
    <summary>SDL 2 库文件的文件夹结构如下（点击收起）：</summary>

<br>

```cmd
> tree /A /F %B4X_DEP_PATH%
.
+---bin
|       SDL2.dll
|
+---include
|   \---SDL2
|           SDL.h
|           ...
|
\---lib
        SDL2.lib
        SDL2main.lib
```

</details>

<br>

#### 3.3.2. FFmpeg 4

1. 从 [FFmpeg Windows Builds - gyan.dev § release](https://www.gyan.dev/ffmpeg/builds/#release-section) 下载 ffmpeg-release-full-shared.7z （曾经是 [FFmpeg Builds - Zeranoe](https://ffmpeg.zeranoe.com/builds/) ）；
2. 将各个文件从压缩包中按照类型提取到指定位置：
   1. 提取`\ffmpeg-4...-full_build-shared\include\`下的所有文件夹以及其中的文件到`%B4X_DEP_PATH%\include\`下；
   2. 提取`\ffmpeg-4...-full_build-shared\lib\`下的所有`.lib`文件到`%B4X_DEP_PATH%\lib\`下；
   3. 提取`\ffmpeg-4...-full_build-shared\bin\`下的所有`.dll`文件到`%B4X_DEP_PATH%\bin\`下。

<details open>
    <summary>FFmpeg 4 库文件的文件夹结构如下（点击收起）：</summary>

<br>

```cmd
> tree /A /F %B4X_DEP_PATH%
.
+---bin
|       avcodec-58.dll
|       avdevice-58.dll
|       avfilter-7.dll
|       avformat-58.dll
|       avutil-56.dll
|       postproc-55.dll
|       swresample-3.dll
|       swscale-5.dll
|
+---include
|   +---libavcodec
|   |       avcodec.h
|   |       ...
|   |
|   +---libavdevice
|   |       avdevice.h
|   |       ...
|   |
|   +---libavfilter
|   |       avfilter.h
|   |       ...
|   |
|   +---libavformat
|   |       avformat.h
|   |       ...
|   |
|   +---libavutil
|   |       avutil.h
|   |       ...
|   |
|   +---libpostproc
|   |       postprocess.h
|   |       ...
|   |
|   +---libswresample
|   |       swresample.h
|   |       ...
|   |
|   \---libswscale
|           swscale.h
|           ...
|
\---lib
        avcodec.lib
        avdevice.lib
        avfilter.lib
        avformat.lib
        avutil.lib
        postproc.lib
        swresample.lib
        swscale.lib
```

</details>

#### 3.3.3. OpenSSL 1.1.1

Qt 5 使用的 TLS 实现默认为 OpenSSL ，无法使用 Windows 的 [Schannel SSP](https://docs.microsoft.com/windows-server/security/tls/tls-ssl-schannel-ssp-overview) <sup><a href="#note_12" id="note_12_sup">[12]</a></sup> <sup><a href="#note_13" id="note_13_sup">[13]</a></sup> <sup><a href="#note_14" id="note_14_sup">[14]</a></sup> ，所以 BesLyric-for-X 需要 OpenSSL 的动态链接库文件。这些动态链接库文件在运行时被链接（ Run-time Dynamic Linking ），因此并不能通过 [Dependency Walker](https://www.dependencywalker.com/) 或 [lucasg/Dependencies](https://github.com/lucasg/Dependencies) 等类似工具在 BesLyric-for-X 的导入表中找到相关的信息。

OpenSSL 的 1.0 与 1.1 版本并非二进制兼容 <sup><a href="#note_15" id="note_15_sup-a">[15.a]</a></sup> ，且从 Qt 5.12.4 开始，包含 Qt 5.13.0 在内，所有 Qt 均使用 OpenSSL 1.1.1 <sup><a href="#note_15" id="note_15_sup-b">[15.b]</a></sup> <sup><a href="#note_16" id="note_16_sup">[16]</a></sup> ，所以 BesLyric-for-X 也使用 OpenSSL 1.1.1 。

<br>

由于某些国家和地区的法律对密码学技术的进出口限制 <sup><a href="#note_17" id="note_17_sup">[17]</a></sup> <sup><a href="#note_18" id="note_18_sup">[18]</a></sup> ， Qt 不能（自动，或通过`windeployqt`等途径）将 OpenSSL 一并交付 <sup><a href="#note_19" id="note_19_sup">[19]</a></sup> ，开发者需要自行编译，或者从第三方获得 OpenSSL 库文件 <sup><a href="#note_20" id="note_20_sup">[20]</a></sup> 。

[Binaries - OpenSSLWiki](https://wiki.openssl.org/index.php/Binaries) 上有一个包含一些**第三方**网站链接的表格，这些网站提供了预编译的 OpenSSL 库文件。为了更容易地获取库文件，也为了避免库文件本身的依赖可能造成的问题（例如 [BesLyric-for-X#27](https://github.com/BesLyric-for-X/BesLyric-for-X/issues/27) ），经过对比，最终选择了用于 ICS 项目的 OpenSSL 库文件。

**注意：使用这些未经过 OpenSSL 项目评估或测试的 OpenSSL 的衍生产品，后果自负。** <sup><a href="#note_21" id="note_21_sup">[21]</a></sup>

<br>

- 下载用于 64 位 Windows 的 [OpenSSL](http://wiki.overbyte.eu/wiki/index.php/ICS_Download#Download_OpenSSL_Binaries_.28required_for_SSL-enabled_components.29) ；
- 对于动态库文件，从压缩包中提取`libcrypto-1_1-x64.dll`和`libssl-1_1-x64.dll`文件到`%B4X_DEP_PATH%\win64\bin\`下。

<details open>
    <summary>完整的 OpenSSL 1.1 库文件的文件夹结构如下（点击收起）：</summary>

<br>

```cmd
> tree /A /F %B4X_DEP_PATH%
.
\---bin
       libcrypto-1_1-x64.dll
       libssl-1_1-x64.dll
```

</details>

<br>

## 4. 脚注

1. <a href="#note_1_sup" id="note_1">^</a> Windows 10 - release information - Windows Release Information | Microsoft Docs<br>https://docs.microsoft.com/windows/release-information/
2. <a href="#note_2_sup" id="note_2">^</a> Visual Studio Product Lifecycle and Servicing | Microsoft Docs § Support for older versions of Visual Studio<br>https://docs.microsoft.com/visualstudio/releases/2019/servicing#support-for-older-versions-of-visual-studio
3. <a href="#note_3_sup" id="note_3">^</a> Qt version history - Wikipedia § Qt 5<br>https://en.wikipedia.org/wiki/Qt_version_history#Qt_5
4. <a href="#note_4_sup" id="note_4">^</a> Windows: Get Windows Feature Version/Release ID<br>https://michlstechblog.info/blog/windows-get-windows-feature-version-release-id/
5. <a href="#note_5_sup" id="note_5">^</a> How to Find Your Windows 10 Build Number, Version, Edition and Bitness<br>https://www.winhelponline.com/blog/find-windows-10-build-version-edition-bit/
6. <a href="#note_6_sup" id="note_6">^</a> `Invalid parameter passed to C runtime function.` on Windows 10 1809<br>https://blog.csdn.net/u014132751/article/details/91876385<br>https://bugreports.qt.io/browse/QTBUG-70917<br>https://developercommunity.visualstudio.com/content/problem/363323/getadaptersaddresses-invalid-parameter-passed-to-c.html<br>https://blog.csdn.net/u014132751/article/details/91876385
7. <a href="#note_7_sup" id="note_7">^</a> Specifying Environment Settings | Qt Creator Manual<br>https://doc.qt.io/qtcreator/creator-project-settings-environment.html
8. <a href="#note_8_sup" id="note_8">^</a> *The debugger that comes with Debugging Tools for Windows goes by the name [WinDbg](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-download-tools), short for Windows Debugger, and generally pronounced win-debug or win-dee-bee-gee. It’s part of a suite of lightweight debuggers, along with `ntsd` (short for NT Symbolic Debugger) and `cdb` (Console Debugger), which are all based on the same debugging engine, creatively named `DbgEng`...*<br>How do you pronounce WinDbg? | The Old New Thing<br>https://devblogs.microsoft.com/oldnewthing/20200128-00/?p=103371
9. <a href="#note_9_sup" id="note_9">^</a> *... One thing I would add is if you use the Visual Studio Installer, you get an option to install the latest Windows SDK. However, it seems the debugging tools are not installed by default with the SDK - I had to go to Control Panel -> Programs and Features and change the SDK installation to include the debugging tools.*<br>c++ - Debugging in QtCreator using MSVC2017 compiler - Stack Overflow<br>https://stackoverflow.com/questions/47773289/debugging-in-qtcreator-using-msvc2017-compiler#comment94564781_47773290
10. <a href="#note_10_sup" id="note_10">^</a> C++ binary compatibility 2015-2019 | Microsoft Docs<br>https://docs.microsoft.com/cpp/porting/binary-compat-2015-2017
11. <a href="#note_11_sup" id="note_11">^</a> Qt Tools and Versions &amp; MSVC 2019 | Qt Forum<br>https://forum.qt.io/topic/104123/qt-tools-and-versions-msvc-2019
12. <a href="#note_12_sup" id="note_12">^</a> [QTBUG-62637] [Windows]: Add support for using Secure Channel for SSL sockets - Qt Bug Tracker<br>https://bugreports.qt.io/browse/QTBUG-62637
13. <a href="#note_13_sup" id="note_13">^</a> *Windows: Secure Channel support for SSL socket (QTBUG-62637)*<br>New Features in Qt 5.13 - Qt Wiki<br>https://wiki.qt.io/New_Features_in_Qt_5.13
14. <a href="#note_14_sup" id="note_14">^</a> [QTBUG-82876] [Windows]: Switch default builds to use Secure Channel for SSL sockets - Qt Bug Tracker<br>https://bugreports.qt.io/browse/QTBUG-82876
15. <span id="note_15">^</span> <a href="#note_15_sup-a"><sup><i><b>a</b></i></sup></a> *... Unfortunately OpenSSL 1.1 is binary incompatible with 1.0, so users need to switch to the new one and repackage their applications...*<br><a href="#note_15_sup-b"><sup><i><b>b</b></i></sup></a> *... As an important new item it provides binaries build with OpenSSL 1.1.1, including the new TLS 1.3 functionality.*<br>Qt 5.12.4 Released with support for OpenSSL 1.1.1<br>https://www.qt.io/blog/2019/06/17/qt-5-12-4-released-support-openssl-1-1-1
16. <a href="#note_16_sup" id="note_16">^</a> [Releasing] Meeting minutes from Qt Release Team meeting 26.03.2019<br>https://lists.qt-project.org/pipermail/releasing/2019-March/002614.html
17. <a href="#note_17_sup" id="note_17">^</a> Legal Restrictions on Cryptography - Web Security, Privacy &amp; Commerce, 2nd Edition [Book]<br>~~https://www.oreilly.com/library/view/web-security-privacy/0596000456/ch04s04.html~~ （失效）
18. <a href="#note_18_sup" id="note_18">^</a> *Import and export restrictions apply for some types of software, and for some parts of the world...*<br> Qt 5.13.0 Known Issues - Qt Wiki § OpenSSL<br>https://wiki.qt.io/Qt_5.13.0_Known_Issues#OpenSSL
19. <a href="#note_19_sup" id="note_19">^</a> *... It's not handled because by default, OpenSSL is dlopened and Qt doesn't ship OpenSSL because of international laws about cryptographic software...*<br>How to properly include openssl for a windows deployment when using mingw? | Qt Forum<br>https://forum.qt.io/topic/86420/how-to-properly-include-openssl-for-a-windows-deployment-when-using-mingw/2<br>*By default Qt is build to dlopen the OpenSSL dlls. This allows Qt to be build to support cryptographie while letting the developer handle all related paperwork for its application to be distributed (different countries have different rules regarding cryptographically enabled software).*<br>Windows and OpenSSL Woes | Qt Forum<br>https://forum.qt.io/topic/91255/windows-and-openssl-woes/12
20. <a href="#note_20_sup" id="note_20">^</a> *The application may require additional 3rd-party libraries (for example, database libraries), which are not taken into account by windeployqt...If Qt was configured to link against ICU or OpenSSL, the respective DLL's need to be added to the release folder, too.*<br>Qt for Windows - Deployment | Qt 5.15<br>https://doc.qt.io/qt-5/windows-deployment.html#creating-the-application-package
21. <a href="#note_21_sup" id="note_21">^</a> *Use these OpenSSL derived products at your own risk; these products have not been evaluated or tested by the OpenSSL project.*<br>Binaries - OpenSSLWiki<br>https://wiki.openssl.org/index.php/Binaries

## 5. 参考

- https://doc.qt.io/qt-5/windows.html

## 6. 其他

- 本文档在 VSCode 上使用 [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one) 辅助编辑；
- 本文档中嵌套表格使用 [HTML Table generator - TablesGenerator.com](https://www.tablesgenerator.com/html_tables) 辅助编辑。
