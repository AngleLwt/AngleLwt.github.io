---
layout:     post                    # 使用的布局（不需要改）
title:      Okhttp3结合Retrofit2 实现缓存               # 标题 
subtitle:   Okhttp3结合Retrofit2 实现缓存 #副标题
date:       2020-05-24           # 时间
author:     李文拓                     # 作者
header-img: img/android1.jpg   #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Android笔记
    - 网络请求
---

```
public class ImpClickModerl implements MvpInterface.ClickAngleModel {

    private Context s;

    private void retrofitQuest(final MvpInterface.ClickAngleCallback clickAngleCallback, Context context) {

        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl(ApiFood.foodUrl)
                .addConverterFactory(GsonConverterFactory.create())
                .addCallAdapterFactory(RxJava2CallAdapterFactory.create())
//                设置客户端
                .client(new OkHttpClient.Builder()
                        .addInterceptor(new AngelCacheIntercptor(context))
                        .addNetworkInterceptor(new AngelCacheIntercptor(context))
                        .connectTimeout(5, TimeUnit.SECONDS)
                        .cache(new Cache(new File(s.getCacheDir(), "Cache"), 1024 * 1024 * 10))
                        .build())
                .build();
//        正常书写retrofit
        ApiFood apiFood = retrofit.create(ApiFood.class);
        Observable<FoodBean> food = apiFood.getFood();
        food.subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(new Observer<FoodBean>() {
                    @Override
                    public void onSubscribe(Disposable d) {

                    }

                    @Override
                    public void onNext(FoodBean foodBean) {
                        if (foodBean != null) {
                            clickAngleCallback.onSucces(foodBean);
                        } else {
                            clickAngleCallback.onFail("获取失败");
                        }
                    }

                    @Override
                    public void onError(Throwable e) {
                        Log.d("TAG", "onError: " + e.getMessage());
                    }

                    @Override
                    public void onComplete() {

                    }
                });

    }

    @Override
    public void click(MvpInterface.ClickAngleCallback clickAngleCallback, Context context) {
        retrofitQuest(clickAngleCallback, context);
    }

    class AngelCacheIntercptor implements Interceptor {


        public AngelCacheIntercptor(Context context) {
            s = context;
        }

        @Override
        public Response intercept(Chain chain) throws IOException {
//            获取网络请求
            Request request = chain.request();
//            判断无网时设置缓存协议
            if (!isNetWorkAvaiable(s)) {
                request = request.newBuilder().cacheControl(CacheControl.FORCE_CACHE).build();
            }
//            获得请求结果对象
            Response proceed = chain.proceed(request);
//            如果有网获取网络数据
            if (isNetWorkAvaiable(s)) {
                int maxAge = 0;
                return proceed.newBuilder()
                        .removeHeader("Pragma")
                        .header("Cache-Control", "public ,max-age=" + maxAge)
                        .build();
            } else {
//                无网获取缓存数据
                int maxStale = 15 * 60;
                return proceed.newBuilder()
                        .removeHeader("Pragma")
                        .header("Cache-Control", "public, only-if-cached, max-stale=" + maxStale)
                        .build();

            }

        }

    }

    public boolean isNetWorkAvaiable(Context context) {
        ConnectivityManager connectivityManager = (ConnectivityManager) context.getSystemService(Context.CONNECTIVITY_SERVICE);
        NetworkInfo info = connectivityManager.getActiveNetworkInfo();
        if (info != null) {
            return info.isAvailable();
        }
        return false;
    }


}
```
