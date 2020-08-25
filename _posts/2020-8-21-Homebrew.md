---
layout:     post                    # 使用的布局（不需要改）
title:    Homebrew           # 标题 
subtitle:  副标题
date:       2020-08-22          # 时间
author:     李文拓                     # 作者
header-img: img/post-bg-e2e-ux.jpg #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Mac
---


在Windows系统下，我们一般安装软件就是从网站或者软件中心下载 ***.exe*** 文件，然后点击安装，一步步点点点就完事了。但是在Linux和类Unix系统中安装软件包相对于刚入门的小白来，有时候比较麻烦，在基于Ubuntu的系统中我们有 ***apt-get*** 包管理器，在CentOS系统中有 ***yum*** 包管理器, 这些都是我们常见的，但是在Mac OS X系统中，自带有 ***Homebrew*** 软件包管理器。当然在Mac和Linux系统中也可以直接在软件中心或者软件官方网站下载安装包，比如 ***.rmp*** 、 ***.dmp*** 等安装包也可以像Windows操作系统中一样点击安装。

***Homebrew*** 是一款Mac OS平台下的软件包管理工具，拥有安装、卸载、更新、查看、搜索等很多实用的功能。简单的一条指令，就可以实现包管理，而不用你关心各种依赖和文件路径的情况，十分方便快捷。

## Homebrew 安装 卸载

**macOS安装环境要求:**

*   A 64-bit Intel CPU
*   macOS High Sierra (10.13) (or higher)
*   Command Line Tools (CLT) for Xcode: `xcode-select --install`,[developer.apple.com/downloads](https://links.jianshu.com/go?to=https%3A%2F%2Fdeveloper.apple.com%2Fdownloads) or [Xcode](https://links.jianshu.com/go?to=https%3A%2F%2Fitunes.apple.com%2Fus%2Fapp%2Fxcode%2Fid497799835)
*   A Bourne-compatible shell for installation (e.g. `bash` or `zsh`)

**安装Homebrew命令**，在终端中输入以下命令就安装完成，默认安装目录是 *`/usr/local`*：

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

```

# curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused的几种解决方式


我认为的产生的原因:`安装失败的可能原因是没有初始化Xcode的环境`这个是针对Mac电脑

我在网上找了很多资料，有说打开这个网址`https://raw.githubusercontent.com/Homebrew/install/master/install`，我打不开
所以下面是我的解决办法:

### 1\. 解决方式一

> 1.查看网址

打开网站[](https://links.jianshu.com/go?to=%255Bhttps%3A%2F%2Fwww.ipaddress.com%2F%255D%28https%3A%2F%2Fwww.ipaddress.com%2F%29)[https://www.ipaddress.com/](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.ipaddress.com%2F)
查询一下 `raw.githubusercontent.com`对应的IP 地址

![image](https://upload-images.jianshu.io/upload_images/2838971-2953bb6c23149393.png?imageMogr2/auto-orient/strip|imageView2/2/w/952/format/webp)

> 2.替换系统的`host`文件
> 注意:最好复制一份出来在更改

![image](https://upload-images.jianshu.io/upload_images/2838971-e303a96675349e06.png?imageMogr2/auto-orient/strip|imageView2/2/w/486/format/webp)

![image](https://upload-images.jianshu.io/upload_images/2838971-1962f1fee2021848.png?imageMogr2/auto-orient/strip|imageView2/2/w/488/format/webp)

![image](https://upload-images.jianshu.io/upload_images/2838971-193c4f584aebc9a4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

> 3.然后执行安装
> `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"`

> 解释一下: 这一行`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"`命令，其实是 安装 `Homebrew`的命令，[官网地址](https://links.jianshu.com/go?to=https%3A%2F%2Fbrew.sh)大家可以自行查看。
**卸载Homebrew命令：**

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"

```

## Homebrew 常用命令

术语：*`homebrew`* 有“家酿、自制”的*意思*, *`formula`* 有“配方”的意思，这里我们一般表示软件包。

> 我们以安装 *wget* 软件举例:

1.  列出所有已经安装的 *`formula`*

    ### brew list

2.  查看指定 *`formula`* 的基本信息, 比如目前的版本, 依赖, 安装后注意事项等

    ### brew info wget

3.  搜索指定的 *`formula`*

    ### brew search wget

4.  安装指定的 *`formula`*

    ### brew install wget

5.  更新指定的 *`formula`*

    ### brew update wget

6.  卸载并重新安装指定的 *`formula`*

    ### brew reinstall wget

7.  卸载指定的 *`formula`*

    ### brew uninstall wget

8.  升级所有可以升级的 *`formula`*

    ### brew upgrade

9.  清理过期的文件、版本，如果指定了 *`formula`* , 则清理指定的

    ### brew cleanup wget

10.  查看帮助

    ### brew help

11.  浏览器中打开指定 *`formula`*, 如果没指定则打开Homebrew首页

    ### brew home wget

> 注意

有些系统中如果安装过程中遇到权限问题，需要sudo指令

了解完整详细的brew命令，请[查看官方文档](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.brew.sh%2FManpage)

如果你对 *`formula`* 感兴趣，请参考[Formula Cookbook](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.brew.sh%2FFormula-Cookbook)

* * *


