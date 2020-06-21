---
layout:     post                    # 使用的布局（不需要改）
title:      Matrix及ColorMatrix               # 标题 
subtitle:   Matrix及ColorMatrix图形处理    #副标题
date:       2020-05-25           # 时间
author:     李文拓                     # 作者
header-img: img/android.jpg   #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Android笔记
---

 **一、Matrix**

1.  Matrix简介
    对于一个图片变换的处理，需要Matrix类的支持，它位于"android.graphics.Matrix"包下，是Android提供的一个矩阵工具类，它本身不能对图像或View进行变换，但它可与其他API结合来控制图形、View的变换，如Canvas。Matrix提供了一些方法来控制图片变换：

> *   setTranslate(float dx,float dy)：控制Matrix进行位移。
> *   setSkew(float kx,float ky)：控制Matrix进行倾斜，kx、ky为X、Y方向上的比例。
> *   setSkew(float kx,float ky,float px,float py)：控制Matrix以px、py为轴心进行倾斜，kx、ky为X、Y方向上的倾斜比例。
> *   setRotate(float degrees)：控制Matrix进行depress角度的旋转，轴心为（0,0）。
> *   setRotate(float degrees,float px,float py)：控制Matrix进行depress角度的旋转，轴心为(px,py)。
> *   setScale(float sx,float sy)：设置Matrix进行缩放，sx、sy为X、Y方向上的缩放比例。
> *   setScale(float sx,float sy,float px,float py)：设置Matrix以(px,py)为轴心进行缩放，sx、sy为X、Y方向上的缩放比例。

之前有提过，图片在内存中存放的就是一个一个的像素点，而对于图片的变换主要是处理图片的每个像素点，对每个像素点进行相应的变换，即可完成对图像的变换。上面已经列举了Matrix进行变换的常用方法，下面以几个Demo来讲解一下如何通过Matrix进行变换。

2.  Matrix缩放

```
     /**
     * 缩放图片
     */
    protected void bitmapScale(float x, float y) {
        // 因为要将图片放大，所以要根据放大的尺寸重新创建Bitmap,Android不允许对原图进行处理
        Bitmap afterBitmap = Bitmap.createBitmap(
                (int) (baseBitmap.getWidth() * x),
                (int) (baseBitmap.getHeight() * y), baseBitmap.getConfig());
        Canvas canvas = new Canvas(afterBitmap);
        // 初始化Matrix对象
        Matrix matrix = new Matrix();
        // 根据传入的参数设置缩放比例
        matrix.setScale(x, y);
        // 根据缩放比例，把图片draw到Canvas上
        canvas.drawBitmap(baseBitmap, matrix,paint);
        iv_after.setImageBitmap(afterBitmap);
    }

bitmapScale(2f, 3f);

```

3.  Matrix旋转

```
/**
     * 图片旋转
     */
    protected void bitmapRotate(float degrees) {
        // 创建一个和原图一样大小的图片
        Bitmap afterBitmap = Bitmap.createBitmap(baseBitmap.getWidth(),
                baseBitmap.getHeight(), baseBitmap.getConfig());
        Canvas canvas = new Canvas(afterBitmap);
        Matrix matrix = new Matrix();
        // 根据原图的中心位置旋转
        matrix.setRotate(degrees, baseBitmap.getWidth() / 2,
                baseBitmap.getHeight() / 2);
        canvas.drawBitmap(baseBitmap, matrix, paint);
        iv_after.setImageBitmap(afterBitmap);
    }

bitmapRotate(30f);

```

4.  Matrix位移

```
/**
     * 图片移动
     */
    protected void bitmapTranslate(float dx, float dy) {
        // 需要根据移动的距离来创建图片的拷贝图大小
        Bitmap afterBitmap = Bitmap.createBitmap(
                (int) (baseBitmap.getWidth() + dx),
                (int) (baseBitmap.getHeight() + dy), baseBitmap.getConfig());
        Canvas canvas = new Canvas(afterBitmap);
        Matrix matrix = new Matrix();
        // 设置移动的距离
        matrix.setTranslate(dx, dy);
        canvas.drawBitmap(baseBitmap, matrix, paint);
        iv_after.setImageBitmap(afterBitmap);
    }

 bitmapTranslate(600f, 200f);

```

5.  Matrix倾斜

```
/**
     * 倾斜图片
     */
    protected void bitmapSkew(float dx, float dy) {
        // 根据图片的倾斜比例，计算变换后图片的大小，
        Bitmap afterBitmap = Bitmap.createBitmap(baseBitmap.getWidth()
                + (int) (baseBitmap.getWidth() * dx), baseBitmap.getHeight()
                + (int) (baseBitmap.getHeight() * dy), baseBitmap.getConfig());
        Canvas canvas = new Canvas(afterBitmap);
        Matrix matrix = new Matrix();
        // 设置图片倾斜的比例
        matrix.setSkew(dx, dy);
        canvas.drawBitmap(baseBitmap, matrix, paint);
        iv_after.setImageBitmap(afterBitmap);
    }

bitmapSkew(1f, 7f);

```

6.  Matrix变换注意事项

*   对于一个从BitmapFactory.decodeXxx()方法加载的Bitmap对象而言，它是一个只读的，无法对其进行处理，必须使用Bitmap.createBitmap()方法重新创建一个Bitmap对象的拷贝，才可以对拷贝的Bitmap进行处理。
*   因为图像的变换是针对每一个像素点的，所以有些变换可能发生像素点的丢失，这里需要使用Paint.setAnitiAlias(boolean)设置来消除锯齿，这样图片变换后的效果会好很多。
*   在重新创建一个Bitmap对象的拷贝的时候，需要注意它的宽高，如果设置不妥，很可能变换后的像素点已经移动到"图片之外"去了。

**二、ColorMatrix**

1.  概述
    ColorMatrix,色彩矩阵,是Android中图片色彩处理的关键类,通过它可以实现图片的各种酷炫效果。

2.使用

*   去除红色

```
public static void bitmapNoRed(Bitmap mBitmap,ImageView mIvNew){
        Bitmap bitmap = Bitmap.createBitmap(mBitmap.getWidth(), mBitmap.getHeight(), mBitmap.getConfig());
        //去掉红色
       float[] mMatrix = new float[]{
                0, 0, 0, 0, 0,//红色
                0, 1, 0, 0, 0,//绿色
                0, 0, 1, 0, 0,//蓝色
                0, 0, 0, 1, 0,//透明色
       };
        //色彩矩阵
        ColorMatrix colorMatrix = new ColorMatrix(mMatrix);
        //画板
        Canvas canvas = new Canvas(bitmap);
        //画笔
        Paint paint = new Paint();
        //给画笔设置颜色过滤器,里面使用色彩矩阵
        paint.setColorFilter(new ColorMatrixColorFilter(colorMatrix));
        //将mBitmap临摹到bitmap上,使用含有色彩矩阵的画笔
        canvas.drawBitmap(mBitmap, 0, 0, paint);
        mIvNew.setImageBitmap(bitmap);
    }

bitmapNoRed(baseBitmap);

```

2.  封装的api:
    除了直接设置矩阵的值外，该类还封装了一些API来快速调整矩阵参数，如：通过setRotate（）方法设置色调、setSaturation（）方法设置饱和度、setScale（）方法设置亮度。这些方法使用起来很简单

```
 // 创建副本，用于将处理过的图片展示出来而不影响原图，Android系统也不允许直接修改原图
        Bitmap bmp = Bitmap.createBitmap(bitmap.getWidth(),bitmap.getHeight(), Bitmap.Config.ARGB_8888);
        Canvas canvas = new Canvas(bmp);
        Paint paint = new Paint();

        // 修改色调,即色彩矩阵围绕某种颜色分量旋转
        ColorMatrix rotateMatrix = new ColorMatrix();
        // 0,1,2分别代表像素点颜色矩阵中的Red，Green,Blue分量
        rotateMatrix.setRotate(0,rotate);
        rotateMatrix.setRotate(1,rotate);
        rotateMatrix.setRotate(2,rotate);

        // 修改饱和度
        ColorMatrix saturationMatrix = new ColorMatrix();
        saturationMatrix.setSaturation(saturation);

        // 修改亮度，即某种颜色分量的缩放
        ColorMatrix scaleMatrix = new ColorMatrix();
        // 分别代表三个颜色分量的亮度
        scaleMatrix.setScale(scale,scale,scale,1);

        //将三种效果结合
        ColorMatrix imageMatrix = new ColorMatrix();
        imageMatrix.postConcat(rotateMatrix);
        imageMatrix.postConcat(saturationMatrix);
        imageMatrix.postConcat(scaleMatrix);

        paint.setColorFilter(new ColorMatrixColorFilter(imageMatrix));
        canvas.drawBitmap(bitmap,0,0,paint);

```
