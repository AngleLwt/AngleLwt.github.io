---
layout:     post                    # 使用的布局（不需要改）
title:      RadioButton               # 标题 
subtitle:    RadioButton使用   #副标题
date:       2020-06-28           # 时间
author:     李文拓                     # 作者
header-img: img/android.jpg   #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Android组件
---

# 多选框选中其中一个
```java
       rb1 = (RadioButton) findViewById(R.id.rb1);
        rb2 = (RadioButton) findViewById(R.id.rb2);
        rb3 = (RadioButton) findViewById(R.id.rb3);
        rb1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (rb1.isChecked()==true) {
                    rb2.setChecked(false);
                    rb3.setChecked(false);
                }
            }
        });

        rb2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (rb2.isChecked()==true) {
                    rb1.setChecked(false);
                    rb3.setChecked(false);
                }
            }
        });
        rb3.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (rb3.isChecked()==true) {
                    rb1.setChecked(false);
                    rb2.setChecked(false);
                }
            }
        });

    
```

效果图
![](https://cdn.jsdelivr.net/gh/AngleLwt/images/image/截屏2020-06-28 下午11.32.15.png)