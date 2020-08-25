---
layout:     post                    # 使用的布局（不需要改）
title:    AndroidStudio项目提交到github最详细步骤           # 标题 
subtitle:  
date:       2020-08-22          # 时间
author:     李文拓                     # 作者
header-img: img/post-bg-e2e-ux.jpg #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Git
---

#准备
安装Androidstudio并新建一个工程；

安装git版本控制系统.如Git GUI；

在github网站上注册一个账号.
步骤
1 studio的git配置；

安装好git后启动Androidstudio，打开如下路径File->Settings->Version Control(展开)->git

在Path to Git executable后面的输入框输入你安装的git路径，如下图所示：

![](https://upload-images.jianshu.io/upload_images/21988850-932ebadf0a8e75a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击test按钮如果出现 Git executed successfully 对话框说明配置成功，同时对话框会显示你安装的git版本号；如下图所示
![](https://upload-images.jianshu.io/upload_images/21988850-989e3207b870fe7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开如下路径File->Settings->Version Control(展开)->GitHub，如下图所示
![](https://upload-images.jianshu.io/upload_images/21988850-485a82f167603e3c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

填入如下信息：

Host:github.com

Login:你的github账户名

Password：你的github账户密码

填完之后点击test按钮，如果出现如下对话框说明配置成功，注意，新版的git的储存目录为 D:\Program Files\Git\cmd

3上传工程到github

打开你要上传的工程，顶部菜单选择VCS->Import into Version Control->Share Project on GitHub,如下图所示：
![](https://upload-images.jianshu.io/upload_images/21988850-484ac088d6d18620.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果你是第一次提交该项目会出现如下对话框，提示你这是一个新的存储库（repo），可以自定义repo的名字，和添加描述。
![屏幕快照 2020-03-17 上午10.46.40.png](https://upload-images.jianshu.io/upload_images/21988850-bd2b5bd2809fae1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

填写完毕点击share按钮如果你的工程没有问题会出现如下界
![](https://upload-images.jianshu.io/upload_images/21988850-da4838c215914598.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击Add就完成了




