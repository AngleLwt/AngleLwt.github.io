---
layout:     post                    # 使用的布局（不需要改）
title:     视图绑定取代findViewById              # 标题 
subtitle:  Android 3.6 Binding #副标题
date:       2020-05-10             # 时间
author:     李文拓                      # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Super项目封装
---

# ViewBinding


## 与 findViewById 的区别
与使用 findViewById 相比，视图绑定具有一些很显著的优点：

### Null 安全
由于视图绑定会创建对视图的直接引用，因此不存在因视图 ID 无效而引发 Null 指针异常的风险。此外，如果视图仅出现在布局的某些配置中，则绑定类中包含其引用的字段会使用 @Nullable 标记。
类型安全：每个绑定类中的字段均具有与它们在 XML 文件中引用的视图相匹配的类型。这意味着不存在发生类转换异常的风险。
这些差异意味着布局和代码之间的不兼容将会导致构建在编译时（而非运行时）失败。

### 与数据绑定的对比
视图绑定和数据绑定均会生成可用于直接引用视图的绑定类。但是，视图绑定旨在处理更简单的用例，与数据绑定相比，具有以下优势：

更快的编译速度：视图绑定不需要处理注释，因此编译时间更短。
易于使用：视图绑定不需要特别标记的 XML 布局文件，因此在应用中采用速度更快。在模块中启用视图绑定后，它会自动应用于该模块的所有布局。
反过来，与数据绑定相比，视图绑定也具有以下限制：

视图绑定不支持布局变量或布局表达式，因此不能用于直接在 XML 布局文件中声明动态界面内容。
视图绑定不支持双向数据绑定。
考虑到这些因素，在某些情况下，最好在项目中同时使用视图绑定和数据绑定。您可以在需要高级功能的布局中使用数据绑定，而在不需要高级功能的布局中使用视图绑定。

## 对比使用findviewById的优点
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

* Activity

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

* Fragment

```
    private ResultProfileBinding binding;

    @Override
    public View onCreateView (LayoutInflater inflater,
                              ViewGroup container,
                              Bundle savedInstanceState) {
        binding = ResultProfileBinding.inflate(inflater, container, false);
        View view = binding.getRoot();
        return view;
    }

    @Override
    public void onDestroyView() {
        super.onDestroyView();
        binding = null;
    }
    
```

引用

```
    binding.getName().setText(viewModel.getName());
    binding.button.setOnClickListener(new View.OnClickListener() {
        viewModel.userClicked()
    });
    
```

修改gradle配置文件
`/Applications/Android/Studio.app/Contents/plugins/android/lib/templates/gradle-projects/NewAndroidModule/root/build.gradle.ftl `

# 我将自己写的ViewBinding模版上传至git
下载地址[GitHub](https://github.com/AngleLwt/Android-Studio-Module)

* EmptyActivity
* AngelAcitvity
* MVP

注意：这是studio系统自带模板路径

`/Applications/Android/Studio.app/Contents/plugins/android/lib/templates/activities `

