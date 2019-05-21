# StudyNotes
一.防止部分手机虚拟键遮挡
  fullScreen();
  if (TranslucentNavigationUtils.checkDeviceHasNavigationBar(this)) {
      //防止虚拟键遮挡布局
     TranslucentNavigationUtils.assistActivity(findViewById(android.R.id.content));
  }
 
  
   public void fullScreen(){
        if(null==mImmersionBar){
            mImmersionBar = ImmersionBar.with(this);
        }
        mImmersionBar.statusBarColor(R.color.white)
                .transparentStatusBar()   //状态栏字体是深色，不写默认为亮色
                .fullScreen(true)
                .init();
    }
    private void initImmersinaBar(){
     mImmersionBar = ImmersionBar.with(this);
        if(Build.VERSION.SDK_INT<=Build.VERSION_CODES.KITKAT){
            mImmersionBar.statusBarDarkFont(true,0.2f);  //状态栏字体是深色，不写默认为亮色
        }else{
            mImmersionBar.statusBarDarkFont(true);
        }
        mImmersionBar.statusBarColor(R.color.white)
                .transparentNavigationBar()
                .flymeOSStatusBarFontColor(R.color.black)  //修改flyme OS状态栏字体颜色
                .init();
    }
    
 对状态栏的控制需要导入依赖库： implementation 'com.gyf.barlibrary:barlibrary:2.3.0'
 
 二、一个通用的混淆模板
 
 三、关于android性能、架构和技术问题的探索
 https://www.jianshu.com/p/69141aa52f34
 
 四、android开发的新技术小结
 https://www.zhihu.com/question/32037895
 
五、MVC、MVP、MVVM三种区别及适用场合
https://blog.csdn.net/victoryzn/article/details/78392128

六、MAT内存分析使用
https://blog.csdn.net/itachi85/article/details/77075455

七、高效开发 MVVM 和 databinding 你需要使用的工具
https://juejin.im/post/59ffde2b6fb9a044fe45c03e

八、MVVMHabit 基于谷歌最新AAC架构，MVVM设计模式的一套快速开发库，整合Okhttp+RxJava+Retrofit+Glide等主流模块，满足日常开发需求。使用该框架可以快速开发一个高质量、易维护的Android应用。
https://github.com/goldze/MVVMHabit

九、安卓语言设置
通过NewTask启动的activity语言设置不会生效，所以最好的办法就是在BaseActivity的onCreate方法主动设置一次语言

 public static void setLanguage(Context context,Locale language,String lanCode){
        forceLocale(context,language);
        MyApplication.getInstance().setLanguage(language);
        Log.d(TAG, "setLanguage: "+language.toString());
        MyApplication.getInstance().getLanguageHelper().setValue("lan",lanCode);
    }


    public static void forceLocale(Context context, Locale locale) {
        if(context==null){
            return;
        }
        Configuration conf = context.getResources().getConfiguration();
        updateConfiguration(conf, locale);
        context.getResources().updateConfiguration(conf,  context.getResources().getDisplayMetrics());

        Configuration systemConf = Resources.getSystem().getConfiguration();
        updateConfiguration(systemConf, locale);
        Resources.getSystem().updateConfiguration(conf,  context.getResources().getDisplayMetrics());

        Locale.setDefault(locale);

    }

    public static void updateConfiguration(Configuration conf, Locale locale) {
        if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN_MR1){
            conf.setLocale(locale);
        }else {
            //noinspection deprecation
            conf.locale = locale;
        }
    }
    //languageCode
     public static final String ID_CHINESE="zh_CN";
    public static final String ID_EN="en";
    public static final String ID_HK="zh_HK";
    public static final String ID_TAIWAN= "zh_TW";
    public static final String ID_TAIGUO="th_TH";
    public static final String ID_PHILINPIN="phi";
    public static final String ID_MAYLA="ms_MY";
    public static final String ID_INDONESIA="in_ID";
    public static final String ID_VIETNAM="vi_VN";
    
十、阴影的显示
    
    <?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android" >

    <!-- 阴影 -->

    <item>
        <shape android:shape="rectangle" >

            <gradient
                android:type="linear"
                android:angle="90"
                android:endColor="#00000000"
                android:startColor="#0f000000"
                />

            <corners
                android:bottomLeftRadius="5dp"
                android:bottomRightRadius="5dp"
                android:topLeftRadius="5dp"
                android:topRightRadius="5dp" />
        </shape>
    </item>


    <!-- 背景 -->
    <!-- 背景盖在阴影上面，留出边距让阴影显示出来 -->
    <item
        android:bottom="3dp"
        android:right="3dp"
        android:left="3dp"
        android:top="3dp"
        >
        <shape android:shape="rectangle" >

            <solid android:color="#ffffff"/>

            <corners
                android:bottomLeftRadius="5dp"
                android:bottomRightRadius="5dp"
                android:topLeftRadius="5dp"
                android:topRightRadius="5dp" />
        </shape>
    </item>

</layer-list>

