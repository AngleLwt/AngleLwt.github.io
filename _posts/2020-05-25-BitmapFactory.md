---
layout:     post                    # 使用的布局（不需要改）
title:      BitmapFactory              # 标题 
subtitle:   BitmapFactory基本 #副标题
date:       2020-05-25           # 时间
author:     李文拓                     # 作者
header-img: img/android1.jpg   #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Android笔记
---

*   Bitmap基本概念：

> bitmap是Android系统中的图像处理的重要类之一；
> 通过bitmap我们可以获取到图片的相关信息；
> bitmap文件图像效果好就需要占用越大存储空间；

*   Bitmap的加载方式：

> 1.BitmapFactory.decodeBetyArray();//字节数组
> 2.BitmapFactory.decodeFile();//文件路径
> 3.BitmapFactory.decodeResource();//资源ID
> 4.BitmapFactory.decodeStream();//流

*   Bitmap不做处理的加载方式：

> Bitmap bitmap = BitmapFactory.decodeResource(getResources,R.drawable.ic_launcher,null);
> //最后一个参数是BitmapFactory.Options,bitmap的高效处理就是通过BitmapFactory.Options来设置的

### BitmapFactory.Options的作用：

> 1.防止内存溢出；
> 2.节省内存开销；
> 3.系统更流畅；

### BitmapFactory.Options的重要属性:

1.injustDecodeBounds;

> *   设为true，那么BitmapFactory并不会真的返回一个Bitmap给你，它仅仅会把它的宽，高取回来给你，这样就不会占用太多的内存，也就不会那么频繁的发生OOM。
> *   设置为false，BitmapFactory返回bitmap;

2.outWidth&outHeight;

> *bitmap图像的宽和高；
> dpi/160*100

3.inSampleSize;
获取采样率

*   inSampleSize大于1时，图像高、宽分别以2的inSampleSize次方分之一缩小
*   inSampleSize小于等于1时，图像高、宽不变；

4.inpreferredConfig：

> *   ALPHA_8: 每个像素用占8位,存储的是图像的透明值,占1个字节;
> *   RGB_565:每个像素用占16位,分别为5-R,6-G,5-B通道,占2个字节;
> *   ARGB-4444:每个像素占16位,即每个通道用4位表示,占2个字节;
> *   ARGB_8888:每个像素占32位,每个通道用8位表示,占4个字节;

5.inDither:

> *   是否进行图像抖动处理；

6.inMutable：

> *   如果设置为true,将返回一个mutable的bitmap,可用于修改BitmapFactory加载而来的bitmap.

7.inScale:

> 是否需要放缩位图

BitmapFactory.options的实际应用：

```
        String pathName = "/storage/emulated/0/test1.jpg";
        //width: 263,ImageView实际比较小
        int width = mIv.getWidth();
        int height = mIv.getHeight();
        Log.d(TAG, "imageView width: "+width);

        //不采样时图片的大小:9216000
        Bitmap bitmap = BitmapFactory.decodeFile(pathName);
        Log.d(TAG, "不二次采样bitmap大小: "+bitmap.getAllocationByteCount());
​
        BitmapFactory.Options options = new BitmapFactory.Options();
        //设置为true,加载图片时不会获取到bitmap对象,但是可以拿到图片的宽高
        options.inJustDecodeBounds = true;
        BitmapFactory.decodeFile(pathName,options);
​
       //计算采样率,对图片进行相应的缩放
        int outWidth = options.outWidth;
        int outHeight = options.outHeight;
        Log.d(TAG, "outWidth: "+outWidth+",outHeight:"+outHeight);
​
        float widthRatio = outWidth*1.0f/width;
        float heightRatio = outHeight*1.0f/height;
​
        Log.d(TAG, "widthRatio: "+widthRatio+",heightRatio:"+heightRatio);
​
        float max = Math.max(widthRatio, heightRatio);
        //向上舍入
        int inSampleSize = (int) Math.ceil(max);
​
        Log.d(TAG, "inSampleSize: "+inSampleSize);
        //改为false,因为要获取采样后的图片了
        options.inJustDecodeBounds = false;
        //8
        options.inSampleSize = inSampleSize;
​
        Bitmap bitmap1 = BitmapFactory.decodeFile(pathName, options);
        //采样后图片大小:144000,是采样前图片的inSampleSize*inSampleSize分之一(1/64)
        Log.d(TAG, "二次采样bitmap大小: "+bitmap1.getAllocationByteCount());
        mIv.setImageBitmap(bitmap1);

```
