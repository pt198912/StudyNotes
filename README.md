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

十一、android studio中ndk的配置（javah,ndk-build的配置）

     https://www.jianshu.com/p/2b29fa617b8d

十二、JNI中java和C字符串的相互转换（防止乱码）

    extern "C" jstring convertToJstr(JNIEnv* env,const char* c_str){

        // 1. 先获取String的jclass
        jclass cls_string = env->FindClass("java/lang/String");
        // 2. 获取构造函数的jmethodID
        jmethodID mid_constructor = env->GetMethodID(cls_string, "<init>","([BLjava/lang/String;)V");
        // 3. new一个String对象
        // 创建一个jbyteArray变量
        // 字节数组里是一个个的字节byte即jbyte，
        // jbyte又是signed char的别名，说明jbyte其实就是char字符
        // 那么char* 字符串就是char字符的集合，即jbyte的集合，就是jbyteArray
        jbyteArray bytes = env->NewByteArray(strlen(c_str));
        env->SetByteArrayRegion(bytes, 0, strlen(c_str), (jbyte*)c_str);

        jstring jstr_charset = env->NewStringUTF("UTF-8");
        return (jstring)env->NewObject( cls_string, mid_constructor,
                                        bytes, jstr_charset);

    }
    extern "C" JNIEXPORT jstring JNICALL
    Java_com_example_myapplication_MainActivity_dataConvert(
            JNIEnv* env,
            jobject /* this */,
            jstring jstr) {
        char* bytes=(char*)env->GetStringUTFChars(jstr,JNI_FALSE);
        size_t len=strlen(bytes);
        __android_log_print(ANDROID_LOG_DEBUG,"pengtao","strlen:%d",len);
        __android_log_print(ANDROID_LOG_DEBUG,"pengtao","INFO:%s,length:%d",bytes,len);
        jstring res=convertToJstr(env,bytes);
        env->ReleaseStringUTFChars(jstr,bytes);
        return res;
    }
    
  十三、图像处理Android-GPUImage-Plus
    
  十四、多个recycleview嵌套时宽度显示不全，重新设置一次LayoutParams.MATCH_PARENT即可；多个Recycleview嵌套，第一个item不置顶，只需在recycleview的上层布局或根布局设置android:focusInTouchMode="true"即可
    
  十五、learnopencv一个好的C++ and python Excmples
    
  十六、C++小demo:github项目名称cpp1x-study-resource

  十七、自定义progressbar的背景
  
    <?xml version="1.0" encoding="utf-8"?>
    <layer-list xmlns:android="http://schemas.android.com/apk/res/android" >
         <item android:id="@android:id/background">     
              <shape>
                  <corners android:radius="7dp" />
                  <gradient
                      android:angle="0"
                      android:endColor="#FFDCC7"
                      android:startColor="#FFDCC7" />
              </shape>    
         </item>
         <item android:id="@android:id/secondaryProgress">
                     <scale android:scaleWidth="100%">
                          <shape>
                             <corners android:radius="7dp"/>
                             <gradient android:angle="0"
                                 android:startColor="#fc4a1a"
                                 android:endColor="#f58019"
                                 android:type="linear"/>
                         </shape>
                     </scale>
          </item>
          <item android:id="@android:id/progress">
                      <scale android:scaleWidth="100%">
                           <shape>
                             <corners android:radius="7dp"/>
                             <gradient android:angle="0"
                                 android:startColor="#fc4a1a"
                                 android:endColor="#f58019"
                                 android:type="linear"/>
                         </shape>
                     </scale>
          </item>
    </layer-list>

十八、activity点击空白处隐藏软键盘：在BaseActivity中加入统一处理

     @Override
      public boolean dispatchTouchEvent(MotionEvent ev) {
        if (ev.getAction() == MotionEvent.ACTION_DOWN) {
            // 获得当前得到焦点的View，一般情况下就是EditText（特殊情况就是轨迹求或者实体案件会移动焦点）
            View v = getCurrentFocus();
            if (isShouldHideInput(v, ev)) {
                Log.d(TAG, "hideSoftInput: ");
                hideSoftInput(v.getWindowToken());
            }else{
                Log.d(TAG, "notHideSoftInput: ");
            }
        }
        for(OnDispatchTouchListener listener:mDispacthTOuchListener){
            if(listener!=null){
                listener.dispatchTouchEvent(ev);
            }
        }
        return super.dispatchTouchEvent(ev);
    }

    /**
     * 根据EditText所在坐标和用户点击的坐标相对比，来判断是否隐藏键盘，因为当用户点击EditText时没必要隐藏
     *
     * @param v
     * @param event
     * @return
     */
    private boolean isShouldHideInput(View v, MotionEvent event) {
        if (v != null && (v instanceof EditText)) {
            int[] l = {0, 0};
            v.getLocationOnScreen(l);
            int left = l[0], top = l[1], bottom = top + v.getHeight(), right = left
                    + v.getWidth();
            if (event.getRawX() > left && event.getRawX() < right && event.getRawY() > top && event.getRawY() < bottom) {
                // 点击EditText的事件，忽略它。
                return false;
            } else {
                return true;
            }
        }
        // 如果焦点不是EditText则忽略，这个发生在视图刚绘制完，第一个焦点不在EditView上，和用户用轨迹球选择其他的焦点
        return false;
    }

    /**
     * 多种隐藏软件盘方法的其中一种
     *
     * @param token
     */
    private void hideSoftInput(IBinder token) {
        if (token != null) {
            InputMethodManager im = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
            im.hideSoftInputFromWindow(token, 0);
        }
    }
 
 十九、22个值得收藏的android开源代码-UI篇
 
     http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2014/1020/1808.html
 
 二十、安卓组件化开发
 
     https://www.jianshu.com/p/bfd5afed498f

二十一、使用ImmersionBar后如何让软键盘弹出时将布局顶起来：

    mImmersionBar.keyboardEnable(true).keyboardMode(WindowManager.LayoutParams.SOFT_INPUT_ADJUST_RESIZE)
                .transparentStatusBar().statusBarDarkFont(true,0.2f)
                .init();
                
二十二、《安卓-深入浅出MVVM教程》应用篇

     https://mp.weixin.qq.com/s/TXJ-cZbUxIKhe89YjyxGZw?
     
二十三、android studio不识别C++代码：

     点击File->Link C++ Project with Gradle
     
二十四、Flutter学习库

     Flutter-learning
     
二十五、一个老吊的android和Flutter的学习笔记，包含bat大厂面试题记录

     https://github.com/AweiLoveAndroid
     
二十六、国内镜像仓库
     
     maven { url'http://maven.aliyun.com/nexus/content/repositories/google' }
     maven { url'http://maven.aliyun.com/nexus/content/groups/public/' }
     maven { url'http://maven.aliyun.com/nexus/content/repositories/jcenter'}
     
二十七、蚂蚁免费VPN

     https://invited.antss020.com/aff/zBFH
     
     
二十八、Android Hook 机制之简单实战
     
     https://www.jianshu.com/p/c431ad21f071
