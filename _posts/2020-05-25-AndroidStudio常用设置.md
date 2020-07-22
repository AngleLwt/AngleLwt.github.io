---
layout:     post                    # 使用的布局（不需要改）
title:      Android Studio常用设置               # 标题 
subtitle:   Mac Android Studio #副标题
date:       2020-05-25           # 时间
author:     李文拓                     # 作者
header-img: img/android.jpg   #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Android Studio
---
# Mac模拟器无法联网问题

作为一枚安卓开发者，免不了使用模拟器来开发调试（毕竟它比手机方便太多）。但是因为适配需要，我们会更多的使用真机开发调试，那么当模拟器遇到这样那样的问题，我们往往会无从下手，其中最烦的就是网络问题。我最近图方便，又重新用起了模拟器，但是发现联网失败，用模拟器内置的浏览器也打不开网页，于是百度上一通找，毫无效果，各种说用cmd命令行设置模拟器dns的都没用，显示没有什么dns文件啥的，后来我觉得是不是最近模拟器的配置更新了，没了那种dns配置文件啥的（因为我用getprop命令获取到的配置信息也没有dns那些信息）或者网上那些解决方案仅限于Windows，当然这只是本人的大胆猜测。接下来我来介绍一下谷歌得来的“科学方法”。

解决方案
1.点击右上角WiFi图标 --> 打开网络偏好设置
2.左边菜单栏选中Wi-Fi --> 点击右下角高级
3.顶部菜单栏选中DNS --> 点击左侧(DNS服务器)的+ --> 添加一项"8.8.8.8"，点击好来保存 --> 回到网络界面点应用即可
(如果想保留以前的dns设置，建议按顺序添加多个，如先添加8.8.8.8，再添加x.x.x.x，以此类推)
![屏幕快照 2020-03-26 下午12.20.21.png](https://upload-images.jianshu.io/upload_images/21988850-27f757b321e3534f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.关闭并重启模拟器
PS:
以上仅仅是对于用WiFi无法使用AS中自带模拟器网络的解决方案，理论上宽带的解决方案也是一样的，因为宽带也有DNS设置，附上一张图
![image.png](https://upload-images.jianshu.io/upload_images/21988850-54be8a9d83146fd0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# Android Studio模拟器查找，存入文件问题
使用Device File Explorer

1.打开View-->Tool Windows-->Device File Explorer

在右侧就可以看到模拟器的文件管理器了，在选择框下拉，可以选择已经启动的AVD。
![](https://upload-images.jianshu.io/upload_images/21988850-ee7705cd28e7d164.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.下面列表即为AVD的文件管理

>1.打开mnt文件夹下的sdcard文件夹就找到了
2.选择上传的目录，右键选择Upload，选择本地文.              件，就可以将文件上传到AVD的sdcard里面了。


![](https://upload-images.jianshu.io/upload_images/21988850-29a63ebe4172c154.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# Studio 常用设置
## 常用快捷键

| 按键 | 说明 |
| --- | --- |
| Ctrl+E | 显示最近编辑的文件列表 |
| Ctrl+Q | 查看方法的参数说明 |
| Ctrl+F | 当前类中查找 |
| Ctrl+R | 当前类中替换 |
| Ctrl+P | 显示方法参数信息 |
| Ctrl+D | 复制一行或者复制选中的内容 |
| Ctrl+Y | 删除一行或者删除选中的内容 |
| Ctrl+[或] | 跳到大括号的开头结尾 |
| Ctrl+W | 选中光标所在的内容上，多次按W可以放大选中范围 |
| Ctrl+O | 选择父类的方法进行重写 |
| Ctrl+H | 显示类的继承结构 |
| Ctrl+G | 快速定位的某一行 |
| Ctrl+U | 快速跳转父类或者父类的方法中 |
| 双击Shift | 全局查找文件 |
| Ctrl+Shift+F | 全局搜索(包括文件、类名、方法名、变量名等等) |
| Ctrl+Alt+F | 提一个变量为全局变量 |
| Ctrl+Alt+M | 将选中的内容提升到一个方法中 |
| Ctrl+Alt+L | 格式化代码，是代码更美观 |
| Ctrl+Alt+O | 清除无用的导包 |
| Ctrl+Alt+S | 打开设置 |
| Ctrl+Alt+T | 把代码包在一块内，例如try/catch、if等 |
| Alt+Insert | 生成构造器/Getter/Setter等方法 |
| Shift+F6 | 将选中文件重命名 |
| Shift+F11 | 查看书签 |
| Ctrl+F11 | 添加书签 |

## 常用配置

> 1.  智能感知不区分大小写
>     File —>Settings —>Editor —>General —> Code Completion —>Case sensitive competion (选择None)
     ![image](//upload-images.jianshu.io/upload_images/8901031-46e877f23e3d8595.png?imageMogr2/auto-orient/strip|imageView2/2/w/1152/format/webp)

> 2.  自动导包
>     File —>Settings —>Editor —> General —> Auto Import —>Add unambiguous imports on the fly (勾选则自动导包)
     ![image](//upload-images.jianshu.io/upload_images/8901031-0977f30c8ece3f51.png?imageMogr2/auto-orient/strip|imageView2/2/w/1152/format/webp)

> 3.  是否直接启动最后打开的项目
>     File —>Settings —>Appearance&Behavior—> System Settings —>Reopen last project on startup (取消勾选则进入工程目录，不会直接进入项目)
    ![image](//upload-images.jianshu.io/upload_images/8901031-0f01703c9db27503.png?imageMogr2/auto-orient/strip|imageView2/2/w/1152/format/webp)

> 4.  修改日志窗口字体
>     File —>Settings —>Editor —> Color Scheme —> Console Font
    ![image](//upload-images.jianshu.io/upload_images/8901031-755910418733fefe.png?imageMogr2/auto-orient/strip|imageView2/2/w/1152/format/webp)

> 5.  修改编辑代码窗口字体
>     File —>Settings —>Editor —> Color Scheme —> Console Scheme Font
 ![image](//upload-images.jianshu.io/upload_images/8901031-c437d12f4876d8cc.png?imageMogr2/auto-orient/strip|imageView2/2/w/1152/format/webp)

> 6.  修改系统窗口字体
>     File —>Settings —>Appearance&Behavior—>Appearance
 ![image](//upload-images.jianshu.io/upload_images/8901031-11f0bec5dc502c10.png?imageMogr2/auto-orient/strip|imageView2/2/w/1152/format/webp)

> 7.  护眼模式
>     File —>Settings —>Editor —> Color Scheme —> General
 ![image](//upload-images.jianshu.io/upload_images/8901031-48f528fa59b696a4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1152/format/webp)

# 清空模拟器缓存
您可以对模拟器执行以下操作：

*   要运行使用 AVD 的模拟器，请双击 AVD。或者点击 **Launch** 

    
*   要停止运行的模拟器，请右键点击 AVD，然后选择 **Stop**。或者，点击“Menu”

    ，然后选择 **Stop**。
*   要为模拟器清除数据，并返回到首次定义时的状态，请右键点击 AVD，然后选择 **Wipe Data**。或者，点击“Menu” ，然后选择 **Wipe Data**。
![](https://upload-images.jianshu.io/upload_images/21988850-06bb27532dcd5999.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
