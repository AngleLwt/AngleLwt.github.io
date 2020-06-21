---
layout:     post                    # 使用的布局（不需要改）
title:     视图绑定取代findViewById              # 标题 
subtitle:  Android 3.6 Binding #副标题
date:       2020-05-10             # 时间
author:     李文拓                      # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Android笔记
---
对比使用findviewById的优点
1.Null 安全：并没有R.id.tv 但是还要去findviewByid就会崩溃
2.类型安全：findviewByid一个TextView但是强转成一个imageView就会崩溃
欲使用此功能，请先保证你的android studio 版本不低于3.6
build.gradle 文件中配置 viewBinding 选项:


```
//Android Studio 3.6
android {
    viewBinding {
        enabled = true
    }
}


// Android Studio 4.0
android {
    buildFeatures {
        viewBinding = true
    }
}


```
这个类里面就是对里面所有view的映射

没使用视图绑定：
```
public class MainActivity extends AppCompatActivity {
 @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        TextView textView = findViewById(R.id.text);
        textView.setText("ok");
    }
 }

```
使用了视图绑定:
```
public class MainActivity extends AppCompatActivity {
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ActivityMainBinding root = ActivityMainBinding.inflate(LayoutInflater.from(this));
        setContentView(root.getRoot());
        root.text.setText("ok");
    }
 }
```
修改gradle配置文件
`/Applications/Android/Studio.app/Contents/plugins/android/lib/templates/gradle-projects/NewAndroidModule/root/build.gradle.ftl `

谷歌总结:

findViewById 是许多用户可见 bug 的来源: 我们很容易传入一个布局中根本不存在的 id，从而导致空指针异常而崩溃；由于此方法类型不安全，也很容易使人写出像 findViewById<TextView>(R.id.image) 这样的，导致类型转换错误的代码。为了解决这些问题，视图绑定把 findViewById 替换成了更加简洁和安全的实现。

