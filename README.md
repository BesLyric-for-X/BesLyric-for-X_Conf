<!-- omit in toc -->
# BesLyric-for-X Development Environment Configuration (macOS x64, Chinese only)

<!-- omit in toc -->
## 简介

本仓库中的文档用于 macOS x64 上的 BesLyric-for-X 的开发环境配置。

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议</a>进行许可。

<!-- omit in toc -->
## 导航

返回[主分支](../main/README.md)。

<!-- omit in toc -->
## 注意

- 本文档可能不提供通过编译源码来配置环境的方法；
- 本文档可能不提供有关已结束支持的操作系统和软件的内容；
- BesLyric-for-X 仅支持 Qt 5.12.4 及更高版本。

<!-- omit in toc -->
## 目录

- [1. 有效性参考](#1-有效性参考)
- [2. BesLyric-for-X 项目配置](#2-beslyric-for-x-项目配置)
  - [2.1. 环境变量`PATH`](#21-环境变量path)
- [3. 配置开发环境](#3-配置开发环境)
  - [3.1. 基础设施](#31-基础设施)
    - [3.1.1. Command Line Developer Tools (CLT)](#311-command-line-developer-tools-clt)
    - [3.1.2. Homebrew](#312-homebrew)
  - [3.2. Qt 5.12.4+](#32-qt-5124)
    - [3.2.1. 在线安装程序](#321-在线安装程序)
    - [3.2.2. HomeBrew](#322-homebrew)
  - [3.3. 第三方库](#33-第三方库)
    - [3.3.1. SDL 2, FFmpeg 4 & pkg-config](#331-sdl-2-ffmpeg-4--pkg-config)
- [4. 脚注](#4-脚注)
- [5. 参考](#5-参考)
- [6. 其他](#6-其他)

## 1. 有效性参考

按照本文档进行配置后，通过测试的操作系统与开发环境的版本组合：

| OS & Kernel <sup><a href="#note_1" id="note_1_sup">[1]</a></sup> | Compiler | CLT <sup><a href="#note_2" id="note_2_sup">[2]</a></sup> | Qt | SDL | FFmpeg | SSL Impl. |
|:- |:- |:- |:- |:- |:- |:- |
| Mac OS X 10.15.6 19G73 & Darwin Kernel Version 19.6.0 | Apple clang version 11.0.3 (clang-1103.0.32.62) | 11.5.0.0.1.1588476445 | 5.12.9 | 2.0.12 | ffmpeg version 4.3.1 built with Apple clang version 11.0.3 (clang-1103.0.32.62) | LibreSSL 2.8.3 |
| Mac OS X 10.14.6 18G103 & Darwin Kernel Version 18.7.0 | Apple LLVM version 10.0.1 (clang-1001.0.46.4) | 10.3.0.0.1.1562985497 | 5.12.9 | 2.0.12 | ffmpeg version 4.3.1 built with Apple clang version 11.0.0 (clang-1100.0.33.17) | LibreSSL 2.6.5 |
| Mac OS X 10.13.6 17G65 & Darwin Kernel Version 17.7.0 | Apple LLVM version 10.0.0 (clang-1000.10.44.4) | 10.1.0.0.1.1539992718 | 5.12.9 | 2.0.12 | ffmpeg version 4.3.1 built with Apple LLVM version 10.0.0 (clang-1000.11.45.5) | LibreSSL 2.2.7 |

## 2. BesLyric-for-X 项目配置

### 2.1. 环境变量`PATH`

在 macOS 平台，需要使用 Terminal 启动 Qt Creator 以使其持有的`PATH`环境变量包含 HomeBrew 所安装的二进制程序所在的文件夹的路径`$(brew --prefix)/bin` <sup><a href="#note_3" id="note_3_sup">[3]</a></sup> <sup><a href="#note_4" id="note_4_sup">[4]</a></sup> <sup><a href="#note_5" id="note_5_sup">[5]</a></sup> <sup><a href="#note_6" id="note_6_sup">[6]</a></sup> <sup><a href="#note_7" id="note_7_sup">[7]</a></sup> ，进而使用 HomeBrew 安装的`pkg-config`自动配置 BesLyric-for-X 所需的第三方库。

## 3. 配置开发环境

### 3.1. 基础设施

#### 3.1.1. Command Line Developer Tools (CLT)

Command Line Developer Tools 是一个集合，包含大量的开发工具，例如编译器（ Clang ）、构建工具（ Make ）、解析器（ YACC ）、解释器（ Python ）和版本控制系统（ Git 、 SVN ）。

在 macOS 上进行 Qt 开发仅需安装 CLT ，不需要安装 Xcode 。可通过以下两种方法安装 CLT ：

- 在 Terminal 中输入`xcode-select --install`以进行在线安装 <sup><a href="#note_8" id="note_8_sup">[8]</a></sup> ；
- 从 [More Software Downloads - Apple Developer](https://developer.apple.com/download/more/) 下载离线安装程序。

#### 3.1.2. Homebrew

[Homebrew](https://brew.sh/) 是一个用于 macOS 的包管理器。有以下两种安装方法：

- 使用 cunkai 托管在 Gitee 的 Homebrew 国内安装脚本安装 <sup><a href="#note_9" id="note_9_sup">[9]</a></sup> <sup><a href="#note_10" id="note_10_sup">[10]</a></sup> ：

```console
$ /bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

- 使用 tuna 提供的 formula 索引的镜像 <sup><a href="#note_11" id="note_11_sup">[11]</a></sup> 和二进制预编译包的镜像 <sup><a href="#note_12" id="note_12_sup">[12]</a></sup> （对于 zsh 用户，记得将`~/.bash_profile`替换为`~/.zshenv`，否则需要每次进入操作系统后都执行一次`source ~/.bash_profile` <sup><a href="#note_13" id="note_13_sup">[13]</a></sup> ）。

### 3.2. Qt 5.12.4+

#### 3.2.1. 在线安装程序

从 [Index of /archive/qt](http://download.qt.io/archive/qt/) 下载安装程序。安装时记得勾选用于 macOS 的预编译组件，例如“Qt / Qt 5.15.0 / macOS”。

#### 3.2.2. HomeBrew

安装 Qt 和 Qt Creator：

```console
$ brew install qt && \
    brew cask install qt-creator
```

### 3.3. 第三方库

#### 3.3.1. SDL 2, FFmpeg 4 & pkg-config

安装 SDL 2 、 FFmpeg 4 和 pkg-config ：

```console
$ brew install sdl2 ffmpeg pkg-config
```

安装后能在`/usr/local/include/`和`/usr/local/lib/`下找到相关文件（以`libavcodec`为例）：

```console
$ find -L /usr/local/include /usr/local/lib \( -type f -o -type l \) -name "*avcodec*" -exec ls -l {} \;
-rw-r--r--  1 mac  staff  148465 Jul 11 18:39 /usr/local/include/libavcodec/avcodec.h
lrwxr-xr-x  1 mac  admin  53 Aug 23 12:30 /usr/local/lib/libavcodec.58.91.100.dylib -> ../Cellar/ffmpeg/4.3.1/lib/libavcodec.58.91.100.dylib
lrwxr-xr-x  1 mac  admin  53 Aug 23 12:30 /usr/local/lib/pkgconfig/libavcodec.pc -> ../../Cellar/ffmpeg/4.3.1/lib/pkgconfig/libavcodec.pc
lrwxr-xr-x  1 mac  admin  39 Aug 23 12:30 /usr/local/lib/libavcodec.a -> ../Cellar/ffmpeg/4.3.1/lib/libavcodec.a
lrwxr-xr-x  1 mac  admin  43 Aug 23 12:30 /usr/local/lib/libavcodec.dylib -> ../Cellar/ffmpeg/4.3.1/lib/libavcodec.dylib
lrwxr-xr-x  1 mac  admin  46 Aug 23 12:30 /usr/local/lib/libavcodec.58.dylib -> ../Cellar/ffmpeg/4.3.1/lib/libavcodec.58.dylib
```

也能使用`pkg-config`获取相关的构建参数：

```console
$ pkg-config --cflags --libs libavcodec
-I/usr/local/Cellar/ffmpeg/4.3.1/include -L/usr/local/Cellar/ffmpeg/4.3.1/lib -lavcodec
```

## 4. 脚注

1. <a href="#note_1_sup" id="note_1">^</a> *`sw_vers`*<br>How to check what macOS version you have | Digital Citizen<br>https://www.digitalcitizen.life/macos-version
2. <a href="#note_2_sup" id="note_2">^</a> Determine xcode command line tools version - Ask Different<br>https://apple.stackexchange.com/questions/180957/determine-xcode-command-line-tools-version
3. <a href="#note_3_sup" id="note_3">^</a> brew(1) – The Missing Package Manager for macOS — Homebrew Documentation<br>https://docs.brew.sh/Manpage#--prefix---unbrewed-formula-
4. <a href="#note_4_sup" id="note_4">^</a> Qt Creator doesn&#x27;t recognize PATH-variable on Mac OSX | Qt Forum<br>https://forum.qt.io/topic/5716/qt-creator-doesn-t-recognize-path-variable-on-mac-osx
5. <a href="#note_5_sup" id="note_5">^</a> lion - How can I have Qt Creator to recognize my environment variables? - Ask Different<br>https://apple.stackexchange.com/questions/54765/how-can-i-have-qt-creator-to-recognize-my-environment-variables
6. <a href="#note_6_sup" id="note_6">^</a> Linux environment variable not recognized by qmake in QtCreator - Stack Overflow<br>https://stackoverflow.com/questions/45368010/linux-environment-variable-not-recognized-by-qmake-in-qtcreator
7. <a href="#note_7_sup" id="note_7">^</a> macos - Adding /usr/local/bin (homebrew) to QtCreator search path for pkg-config on Mac OSX - Stack Overflow<br>https://stackoverflow.com/questions/17129349/
8. <a href="#note_8_sup" id="note_8">^</a> How to Install Command Line Tools in Mac OS X (Without Xcode)<br>https://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/
9. <a href="#note_9_sup" id="note_9">^</a> Homebrew国内如何自动安装（国内地址） - 知乎<br>https://zhuanlan.zhihu.com/p/111014448
10. <a href="#note_10_sup" id="note_10">^</a> HomebrewCN: Homebrew 国内安装脚本<br>https://gitee.com/cunkai/HomebrewCN/
11. <a href="#note_11_sup" id="note_11">^</a> homebrew | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror<br>https://mirror.tuna.tsinghua.edu.cn/help/homebrew/
12. <a href="#note_12_sup" id="note_12">^</a> homebrew-bottles | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror<br>https://mirrors.tuna.tsinghua.edu.cn/help/homebrew-bottles/
13. <a href="#note_13_sup" id="note_13">^</a> *Yes, it's called `~/.zshenv`...*<br>Is there anything in Zsh like bash_profile? - Stack Overflow<br>https://stackoverflow.com/questions/23090390/is-there-anything-in-zsh-like-bash-profile#answer-23091184

## 5. 参考

- https://doc.qt.io/qt-5/macos.html
- https://www.macissues.com/2014/05/26/what-are-the-command-line-developer-tools-in-os-x/
- https://gist.github.com/shoogle/750a330c851bd1a924dfe1346b0b4a08

## 6. 其他

- 本文档在 VSCode 上使用 [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one) 辅助编辑。
