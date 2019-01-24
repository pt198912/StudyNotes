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
