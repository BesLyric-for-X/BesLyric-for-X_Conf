<!-- omit in toc -->
# BesLyric-for-X Development Environment Configuration (Linux x64, Chinese only)

<!-- omit in toc -->
## 简介

本仓库中的文档用于某些 Linux x64 发行版上的 BesLyric-for-X 的开发环境配置。

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议</a>进行许可。

<!-- omit in toc -->
## 导航

返回[主分支](../main/README.md)。

<!-- omit in toc -->
## 注意

- 本文档可能不提供通过编译源码来配置环境的方法；
- 本文档可能不提供有关已结束支持的操作系统 <sup><a href="#note_1" id="note_1_sup">[1]</a></sup> <sup><a href="#note_2" id="note_2_sup">[2]</a></sup> <sup><a href="#note_3" id="note_3_sup">[3]</a></sup> 和软件的内容；
- BesLyric-for-X 仅支持 Qt 5.12.4 及更高版本。

<!-- omit in toc -->
## 目录

- [1. 有效性参考](#1-有效性参考)
- [2. BesLyric-for-X 项目配置](#2-beslyric-for-x-项目配置)
- [3. 配置开发环境](#3-配置开发环境)
  - [3.1. Git 与 C++ 开发环境](#31-git-与-c-开发环境)
    - [3.1.1. Ubuntu 18.04 及更高版本](#311-ubuntu-1804-及更高版本)
    - [3.1.2. Fedora 32](#312-fedora-32)
    - [3.1.3. Manjaro](#313-manjaro)
    - [3.1.4. openSUSE Leap 15.2](#314-opensuse-leap-152)
  - [3.2. Qt 5.12.4 及更高版本](#32-qt-5124-及更高版本)
    - [3.2.1. 包管理器](#321-包管理器)
      - [3.2.1.1. Ubuntu 20.04 及更高版本](#3211-ubuntu-2004-及更高版本)
      - [3.2.1.2. Fedora 32](#3212-fedora-32)
      - [3.2.1.3. Manjaro](#3213-manjaro)
      - [3.2.1.4. openSUSE Leap 15.2](#3214-opensuse-leap-152)
    - [3.2.2. 在线安装程序](#322-在线安装程序)
  - [3.3. 第三方库](#33-第三方库)
    - [3.3.1. OpenGL](#331-opengl)
      - [3.3.1.1. Ubuntu 18.04 及更高版本](#3311-ubuntu-1804-及更高版本)
      - [3.3.1.2. Fedora 32](#3312-fedora-32)
      - [3.3.1.3. Manjaro](#3313-manjaro)
      - [3.3.1.4. openSUSE Leap 15.2](#3314-opensuse-leap-152)
    - [3.3.2. SDL 2](#332-sdl-2)
      - [3.3.2.1. Ubuntu 18.04 及更高版本](#3321-ubuntu-1804-及更高版本)
      - [3.3.2.2. Fedora 32](#3322-fedora-32)
      - [3.3.2.3. Manjaro](#3323-manjaro)
      - [3.3.2.4. openSUSE Leap 15.2](#3324-opensuse-leap-152)
    - [3.3.3. FFmpeg 4](#333-ffmpeg-4)
      - [3.3.3.1. Ubuntu 20.04 及更高版本](#3331-ubuntu-2004-及更高版本)
      - [3.3.3.2. Fedora 32](#3332-fedora-32)
      - [3.3.3.3. Manjaro](#3333-manjaro)
      - [3.3.3.4. openSUSE Leap 15.2](#3334-opensuse-leap-152)
      - [3.3.3.5. Ubuntu 18.04](#3335-ubuntu-1804)
- [4. 异常处理](#4-异常处理)
  - [4.1. 缺少用于 Qt 5.15 的 libxcb](#41-缺少用于-qt-515-的-libxcb)
- [5. 脚注](#5-脚注)
- [6. 参考](#6-参考)
- [7. 其他](#7-其他)

## 1. 有效性参考

按照本文档进行配置后，通过测试的操作系统与开发环境的版本组合：

| OS & Kernel | Compiler | Qt | SDL | FFmpeg | TLS Impl. |
|:- |:- |:- |:- |:- |:- |
| Ubuntu 20.04 LTS &amp; 5.4.0-40-generic | 9.3.0 (Ubuntu 9.3.0-10ubuntu2) | <table><tr><td>5.12.8 (apt)</td></tr><tr><td>5.12.9</td></tr><tr><td>5.15.0</td></tr></table> | 2.0.10 | 4.2.2-1ubuntu1 | OpenSSL 1.1.1f  31 Mar 2020 |
| Fedora release 32 (Thirty Two) &amp; 5.7.16-200.fc32.x86_64 | 10.2.1 20200723 (Red Hat 10.2.1-1) (GCC) | <table><tr><td>5.12.9</td></tr><tr><td>5.13.2 (dnf)</td></tr></table> | 2.0.12 | 4.2.4 | OpenSSL 1.1.1g FIPS  21 Apr 2020 |
| Manjaro Linux xfce 20.0.3 &amp; 5.6.19-2-MANJARO | 10.1.0 (GCC) | <table><tr><td>5.15.0</td></tr><tr><td>5.15.0 (pacman)</td></tr></table> | 2.0.12 | n4.2.4 | OpenSSL 1.1.1g  21 Apr 2020 |
| openSUSE Leap 15.2 &amp; 5.3.18-lp152.33-default | 7.5.0 (SUSE Linux) | <table><tr><td>5.12.7 (zypper)</td></tr><tr><td>5.15.0</td></tr></table> | 2.0.8 | 4.2.1-lp152.1.4 | OpenSSL 1.1.1d  10 Sep 2019 |
| Ubuntu 18.04.4 LTS &amp; 5.3.0-62-generic | 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04) | <table><tr><td>5.12.9</td></tr><tr><td>5.14.2</td></tr><tr><td>5.15.0</td></tr></table> | 2.0.8 | 4.3-2~18.04.york0 | OpenSSL 1.1.1  11 Sep 2018 |

## 2. BesLyric-for-X 项目配置

不需要对项目进行额外的配置。

## 3. 配置开发环境

### 3.1. Git 与 C++ 开发环境

#### 3.1.1. Ubuntu 18.04 及更高版本

```console
# apt update && \
    apt install build-essential pkg-config git
```

#### 3.1.2. Fedora 32

```console
# dnf check-update && \
    dnf groupinstall \
      "Development Tools" \
      "C Development Tools and Libraries"
```

如有需要，可以启用“ FastestMirror ”插件 <sup><a href="#note_4" id="note_4_sup">[4]</a></sup> <sup><a href="#note_5" id="note_5_sup">[5]</a></sup> 使`dnf`选择最低时延的镜像服务器。

#### 3.1.3. Manjaro

```console
# pacman --sync --refresh && \
    pacman --sync base-devel gdb git
```

#### 3.1.4. openSUSE Leap 15.2

```console
# zypper refresh && \
    zypper install --type pattern devel_C_C++ && \
    zypper install pkg-config git
```

### 3.2. Qt 5.12.4 及更高版本

#### 3.2.1. 包管理器

##### 3.2.1.1. Ubuntu 20.04 及更高版本

```console
# apt install git qt5-default qtcreator
```

##### 3.2.1.2. Fedora 32

```console
# dnf install qt5-devel qt-creator
```

##### 3.2.1.3. Manjaro

```console
# pacman --sync qt5-base qtcreator
```

##### 3.2.1.4. openSUSE Leap 15.2

```console
# zypper install --type pattern devel_qt5
```

#### 3.2.2. 在线安装程序

- 从 [Index of /official_releases/online_installers](http://download.qt.io/official_releases/online_installers/) 下载在线安装程序；
- 安装时记得勾选当前编译环境对应的预构建组件（ Pre-built Components ），例如“ Qt / Qt 5.15.0 / Desktop gcc 64-bit ”。

### 3.3. 第三方库

#### 3.3.1. OpenGL

##### 3.3.1.1. Ubuntu 18.04 及更高版本

```console
# apt install libgl1-mesa-dev
```

##### 3.3.1.2. Fedora 32

```console
# dnf install mesa-libGL-devel
```

##### 3.3.1.3. Manjaro

```console
# pacman --sync mesa
```

##### 3.3.1.4. openSUSE Leap 15.2

```console
# zypper install Mesa-libGL-devel
```

#### 3.3.2. SDL 2

##### 3.3.2.1. Ubuntu 18.04 及更高版本

```console
# apt install libsdl2-dev
```

##### 3.3.2.2. Fedora 32

```console
# dnf install SDL2-devel
```

##### 3.3.2.3. Manjaro

```console
# pacman --sync sdl2
```

##### 3.3.2.4. openSUSE Leap 15.2

```console
# zypper install libSDL2-devel
```

#### 3.3.3. FFmpeg 4

##### 3.3.3.1. Ubuntu 20.04 及更高版本

```console
# apt install \
    ffmpeg \
    libavcodec-dev \
    libavdevice-dev \
    libavfilter-dev \
    libavformat-dev \
    libavutil-dev \
    libpostproc-dev \
    libswresample-dev \
    libswscale-dev
```

##### 3.3.3.2. Fedora 32

<sup><a href="#note_6" id="note_6_sup">[6]</a></sup>

```console
# dnf install \
    https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm && \
    dnf install ffmpeg-devel
```

##### 3.3.3.3. Manjaro

```console
# pacman --sync ffmpeg
```

##### 3.3.3.4. openSUSE Leap 15.2

```console
# zypper install \
    ffmpeg-4-libavcodec-devel \
    ffmpeg-4-libavdevice-devel \
    ffmpeg-4-libavfilter-devel \
    ffmpeg-4-libavformat-devel \
    ffmpeg-4-libavutil-devel \
    ffmpeg-4-libpostproc-devel \
    ffmpeg-4-libswresample-devel \
    ffmpeg-4-libswscale-devel
```

##### 3.3.3.5. Ubuntu 18.04

<sup><a href="#note_7" id="note_7_sup">[7]</a></sup>

添加 PPA ：

```console
# add-apt-repository ppa:jonathonf/ffmpeg-4
```

如果 PPA 访问缓慢，可以使用 ustclug 提供的反向代理 <sup><a href="#note_8" id="note_8_sup">[8]</a></sup> 。有人提供了一个替换 URL 的正则表达式 <sup><a href="#note_9" id="note_9_sup">[9]</a></sup> ，我对其进行了一些修改使其仅使用 https ：

```console
# find /etc/apt/sources.list.d/ -type f -name "*.list" -exec \
    sed -i.bak -r 's#deb(-src)?\s*http(s)?://ppa.launchpad.net#deb\1 https://launchpad.proxy.ustclug.org#ig' {} \;
```

安装：

```console
# apt install \
    ffmpeg \
    libavcodec-dev \
    libavdevice-dev \
    libavformat-dev \
    libavfilter-dev \
    libavutil-dev \
    libpostproc-dev \
    libswresample-dev \
    libswscale-dev
```

## 4. 异常处理

### 4.1. 缺少用于 Qt 5.15 的 libxcb

<sup><a href="#note_10" id="note_10_sup">[10]</a></sup> <sup><a href="#note_11" id="note_11_sup">[11]</a></sup> <sup><a href="#note_12" id="note_12_sup">[12]</a></sup> <sup><a href="#note_13" id="note_13_sup">[13]</a></sup> <sup><a href="#note_14" id="note_14_sup">[14]</a></sup> <sup><a href="#note_15" id="note_15_sup">[15]</a></sup>

运行程序时出现错误：

```console
$ ./binary
qt.qpa.plugin: Could not load the Qt platform plugin "xcb" in "" even though it was found.
This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem.

Available platform plugins are: eglfs, linuxfb, minimal, minimalegl, offscreen, vnc, wayland-egl, wayland, wayland-xcomposite-egl, wayland-xcomposite-glx, xcb.

Aborted (core dumped)
```

设置环境变量`QT_DEBUG_PLUGINS=1`再次运行程序：

```console
$ QT_DEBUG_PLUGINS=1 ./binary
QFactoryLoader::QFactoryLoader() checking directory path "/the/path/to/Qt/5.15.0/gcc_64/plugins/platforms" ...

...

Got keys from plugin meta data ("xcb")

...

Cannot load library /the/path/to/Qt/5.15.0/gcc_64/plugins/platforms/libqxcb.so: (libxcb-xinerama.so.0: cannot open shared object file: No such file or directory)
QLibraryPrivate::loadPlugin failed on "/the/path/to/Qt/5.15.0/gcc_64/plugins/platforms/libqxcb.so" : "Cannot load library /the/path/to/Qt/5.15.0/gcc_64/plugins/platforms/libqxcb.so: (libxcb-xinerama.so.0: cannot open shared object file: No such file or directory)"

...

Aborted (core dumped)
```

系统中缺少 libxcb-xinerama ，搜索：

```console
$ apt search libxcb-xinerama
Sorting... Done
Full Text Search... Done
libxcb-xinerama0/focal 1.14-2 amd64
  X C Binding, xinerama extension

libxcb-xinerama0-dev/focal 1.14-2 amd64
  X C Binding, xinerama extension, development files
```

安装：

```console
# apt install libxcb-xinerama0
```

如果缺少其他的库，也按照上述方法进行安装。

## 5. 脚注

1. <a href="#note_1_sup" id="note_1">^</a> Ubuntu release cycle | Ubuntu<br>https://ubuntu.com/about/release-cycle
2. <a href="#note_2_sup" id="note_2">^</a> End of life - Fedora Project Wiki § Unsupported_Fedora_Releases<br>https://fedoraproject.org/wiki/End_of_life#Unsupported_Fedora_Releases
3. <a href="#note_3_sup" id="note_3">^</a> Lifetime - openSUSE Wiki § Discontinued distributions<br>https://en.opensuse.org/Lifetime#Discontinued_distributions
4. <a href="#note_4_sup" id="note_4">^</a> PackageManagement/Yum/FastestMirror - CentOS Wiki<br>https://wiki.centos.org/PackageManagement/Yum/FastestMirror
5. <a href="#note_5_sup" id="note_5">^</a> Dnf fastest mirror on Fedora/CentOS/RHEL &#x2d; Darryl Dias<br>https://darryldias.me/2020/how-to-setup-fastest-mirror-in-dnf/
6. <a href="#note_6_sup" id="note_6">^</a> Enabling the RPM Fusion repositories :: Fedora Docs Site<br>https://docs.fedoraproject.org/en-US/quick-docs/setup_rpmfusion/
7. <a href="#note_7_sup" id="note_7">^</a> How to Install FFmpeg on Ubuntu 18.04 - Linux4one<br>https://linux4one.com/how-to-install-ffmpeg-on-ubuntu-18-04/
8. <a href="#note_8_sup" id="note_8">^</a> Revproxy - LUG @ USTC<br>https://lug.ustc.edu.cn/wiki/mirrors/help/revproxy
9. <a href="#note_9_sup" id="note_9">^</a> Issue #43 (comment) · ustclug/mirrorrequest<br>https://github.com/ustclug/mirrorrequest/issues/43#issuecomment-280996651
10. <a href="#note_10_sup" id="note_10">^</a> qt.qpa.plugin: Could not load the Qt platform plugin &quot;xcb&quot; in &quot;&quot; even though it was found. | Qt Forum<br>https://forum.qt.io/topic/93247/qt-qpa-plugin-could-not-load-the-qt-platform-plugin-xcb-in-even-though-it-was-found
11. <a href="#note_11_sup" id="note_11">^</a> Qt Creator + Ubuntu 20.04 | Qt Forum<br>https://forum.qt.io/topic/116299/qt-creator-ubuntu-20-04
12. <a href="#note_12_sup" id="note_12">^</a> &quot;Failed to load platform plugin &quot;xcb&quot; &quot; while launching qt5 app on linux without qt installed - Stack Overflow<br>https://stackoverflow.com/questions/17106315/failed-to-load-platform-plugin-xcb-while-launching-qt5-app-on-linux-without
13. <a href="#note_13_sup" id="note_13">^</a> “Failed to load platform plugin ”xcb“ ” while launching qt5 app on linux without qt installed - Ask Ubuntu<br>https://askubuntu.com/questions/308128/failed-to-load-platform-plugin-xcb-while-launching-qt5-app-on-linux-without
14. <a href="#note_14_sup" id="note_14">^</a> Identify pyqt5 515 ci issue by j9ac9k · Pull Request #1221 · pyqtgraph/pyqtgraph<br>https://github.com/pyqtgraph/pyqtgraph/pull/1221
15. <a href="#note_15_sup" id="note_15">^</a> Re: [Interest] Qt 5.15 for Linux/X11 : "-qt-xcb" no longer	supported?<br>https://www.mail-archive.com/interest@qt-project.org/msg34052.html

## 6. 参考

- https://doc.qt.io/qt-5/linux.html

## 7. 其他

- 本文档在 VSCode 上使用 [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one) 辅助编辑；
- 本文档中嵌套表格使用 [HTML Table generator - TablesGenerator.com](https://www.tablesgenerator.com/html_tables) 辅助编辑。
