---
layout:     post                    # 使用的布局（不需要改）
title:      PoPwindow_Notification               # 标题 
subtitle:   PoPwindow_Notification实现代码 #副标题
date:       2020-05-13           # 时间
author:     李文拓                     # 作者
header-img: img/android.jpg   #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Android笔记 
---

`实现代码如下`



```

{

    private Button bt_PoPwindow;
    private Button bt_Notification;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initView();
    }

    private void initView() {
        bt_PoPwindow = (Button) findViewById(R.id.bt_PoPwindow);
        bt_PoPwindow.setOnClickListener(this);
        bt_Notification = (Button) findViewById(R.id.bt_Notification);
        bt_Notification.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.bt_PoPwindow:
//                弹窗
                popwindow();
                break;
            case R.id.bt_Notification:
//                通知
                notification();
                break;
        }
    }

    private void notification() {
//       创建通知管理器
        NotificationManager manager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
        String channelId = "1";
        String channelName = "default";

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            NotificationChannel channel = new NotificationChannel(channelId, channelName, NotificationManager.IMPORTANCE_DEFAULT);
            channel.enableLights(true);//开启指示灯,如果设备有的话。
            channel.setLightColor(Color.RED);//设置指示灯颜色
            channel.setShowBadge(true);//检测是否显示角标
            manager.createNotificationChannel(channel);
        }
//        设置传值
        Intent intent = new Intent(this, MainActivity.class);
//      设置padingintent
        PendingIntent activity = PendingIntent.getActivity(this, 100, intent, PendingIntent.FLAG_UPDATE_CURRENT);

        Notification build = new NotificationCompat.Builder(this, channelId)
                .setSmallIcon(R.drawable.gogle)//设置小图
//                .setLargeIcon(BitmapFactory.decodeResource(getResources(), R.drawable.gogle))//设置大
                .setContentText("我是内容")//设置内容
                .setContentTitle("我是标题")//设置标题
                .setContentIntent(activity)//设置延时意图
                .setAutoCancel(true)//设置点击自动消失
                .setNumber(1)//设置角标计数
                .build();
//        发送通知
        manager.notify(100, build);


    }

    //  创建方法
    private void popwindow() {
        View view = LayoutInflater.from(MainActivity.this).inflate(R.layout.pop_one, null);
//        设置大小
        PopupWindow popupWindow = new PopupWindow(view, ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT);
//        点击范围外关闭pop——window
        popupWindow.setBackgroundDrawable(new ColorDrawable());
        popupWindow.setOutsideTouchable(true);
//       设置透明度
        setBackgroundAlpha(0.5f);
//     设置动画
        popupWindow.setAnimationStyle(R.style.PopAnimation);
//        聚焦
        popupWindow.setFocusable(true);
//        展示位置
        popupWindow.showAsDropDown(bt_PoPwindow, 20, 20, Gravity.CENTER_HORIZONTAL);
        popupWindow.setOnDismissListener(new PopupWindow.OnDismissListener() {
            @Override
            public void onDismiss() {
                setBackgroundAlpha(1);
            }
        });
    }

    private void setBackgroundAlpha(float alpha) {
//        获得窗口
        Window window = getWindow();
//        获得窗口属性
        WindowManager.LayoutParams attributes = window.getAttributes();
        attributes.alpha = alpha;
        window.setAttributes(attributes);
    }
}
