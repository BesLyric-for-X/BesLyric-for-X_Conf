<!-- omit in toc -->
# BesLyric-for-X Development Environment Configuration (Windows Mingw-w64 x64, Chinese only)

<!-- omit in toc -->
## 简介

本仓库中的文档用于 Windows Mingw-w64 x64 上的 BesLyric-for-X 的开发环境配置。

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议</a>进行许可。

<!-- omit in toc -->
## 导航

返回[主分支](../main/README.md)。

<!-- omit in toc -->
## 注意

- 本文档可能不提供通过编译源码来配置环境的方法；
- 本文档可能不提供有关已结束支持的操作系统和软件的内容；
- BesLyric-for-X 仅支持 Qt 5.12.4 及更高版本；
- 目前， Mingw-w64 不用于发行版。

<!-- omit in toc -->
## 目录

- [0. 有效性参考](#0-有效性参考)
- [1. BesLyric-for-X 项目配置](#1-beslyric-for-x-项目配置)
  - [1.0. 环境变量](#10-环境变量)
  - [1.1. 构建目标](#11-构建目标)
- [2. 配置开发环境](#2-配置开发环境)
  - [2.0. 基础设施](#20-基础设施)
    - [2.0.0. MSYS2](#200-msys2)
    - [2.0.1. C++ 开发环境](#201-c-开发环境)
  - [2.1. Qt 5.12.4+](#21-qt-5124)
  - [2.2. 第三方库](#22-第三方库)
    - [2.2.0. 多媒体](#220-多媒体)
      - [2.2.0.0. SDL 2](#2200-sdl-2)
      - [2.2.0.1. FFmpeg 4](#2201-ffmpeg-4)
    - [2.2.1. 网络](#221-网络)
      - [2.2.1.0. OpenSSL](#2210-openssl)
- [3. 脚注](#3-脚注)
- [4. 参考](#4-参考)
- [5. 其他](#5-其他)

## 0. 有效性参考

按照本文档进行配置后，通过测试的操作系统与开发环境的版本组合：

| OS &<br>ReleaseID<a href="#note_0" id="note_0_sup"><sup>[0]</sup></a> &<br>Kernel<a href="#note_1" id="note_1_sup"><sup>[1]</sup></a> | Compiler | Qt | SDL | FFmpeg | TLS Impl. |
|:- |:- |:- |:- |:- |:- |
| <table><tr><td>Microsoft Windows 10 Pro &amp;<br>2004 &amp;<br>Microsoft Windows [Version 10.0.19041.388]</td></tr></table> | gcc version 10.2.0 (Rev1, Built by MSYS2 project) | 5.15.0 | 2.0.12 | ffmpeg version 4.3.1 built with gcc 10.2.0 (Rev1, Built by MSYS2 project) | OpenSSL 1.1.1g  21 Apr 2020 |

## 1. BesLyric-for-X 项目配置

### 1.0. 环境变量

BesLyric-for-X 需要一些第三方库。为了正确配置第三方库，在 Windows Mingw-w64 平台，开发 BesLyric-for-X 可能需要环境变量`B4X_DEP_PATH`。

BesLyric-for-X 中的 qmake 指令将尝试对`B4X_DEP_PATH`进行配置。在不改变任何系统设置的情况下，不需要手动配置`B4X_DEP_PATH`。

典型的`B4X_DEP_PATH`值为`C:\msys64\mingw64`，这与 MSYS2 的路径有关。`B4X_DEP_PATH`指向的文件夹下的`bin/`文件夹应该包含所有需要的应用程序和动态链接库。

不建议将该环境变量配置到操作系统或当前用户中，而是配置到项目内（通过 Qt Creator 左侧列表切换到“ Project ”页面，在“ Build Settings ”-“ Build Environment ”或“ Project Settings ”-“ Environment ”中配置环境变量）。

注意：不需要在`B4X_DEP_PATH`指向的文件夹的路径两端加上引号。

### 1.1. 构建目标

有 3 个构建目标可供使用：

- `deploy_3rd_party_library_dlls`
- `windeployqt_enhanced`
- `deploy_windows_dlls`

其中，`deploy_windows_dlls`目标依赖于其他所有的目标，且上述所有目标均依赖于`first`目标。

## 2. 配置开发环境

### 2.0. 基础设施

#### 2.0.0. MSYS2

从 [Index of /distrib/x86_64/](http://repo.msys2.org/distrib/x86_64/) 下载 MSYS2 并进行相关配置<a href="#note_2" id="note_2_sup"><sup>[2]</sup></a>。

#### 2.0.1. C++ 开发环境

```console
$ pacman --sync \
    mingw-w64-x86_64-gcc \
    mingw-w64-x86_64-gdb \
    mingw-w64-x86_64-make \
    mingw-w64-x86_64-pkg-config
```

### 2.1. Qt 5.12.4+

```console
$ pacman --sync \
    mingw-w64-x86_64-qt5 \
    mingw-w64-x86_64-qt-creator
```

### 2.2. 第三方库

#### 2.2.0. 多媒体

##### 2.2.0.0. SDL 2

```console
$ pacman --sync mingw-w64-x86_64-SDL2
```

##### 2.2.0.1. FFmpeg 4

```console
$ pacman --sync mingw-w64-x86_64-ffmpeg
```

#### 2.2.1. 网络

##### 2.2.1.0. OpenSSL

```console
$ pacman --sync mingw-w64-x86_64-openssl
```

## 3. 脚注

0. <a href="#note_0_sup" id="note_0">^</a> Windows: Get Windows Feature Version/Release ID<br>https://michlstechblog.info/blog/windows-get-windows-feature-version-release-id/
1. <a href="#note_1_sup" id="note_1">^</a> How to Find Your Windows 10 Build Number, Version, Edition and Bitness<br>https://www.winhelponline.com/blog/find-windows-10-build-version-edition-bit/
2. <a href="#note_2_sup" id="note_2">^</a> MSYS2 § Installation<br>https://www.msys2.org/#installation

## 4. 参考

- https://www.archlinux.org/pacman/pacman.8.html

## 5. 其他

- 本文档在 VSCode 上使用 [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one) 辅助编辑；
