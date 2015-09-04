安安租开发须知：

1：screenOrientation 属性已经放在主题里面，注册activity的时候不需要写

2：windowsoftinputmode属性已经默认adjustresize ，没有特别需求最好不要使用adjustpan
======================================================================================================================================

（普通的TOOLBAR使用）
1.在布局立面include进appbar.xml文件即可完成title的布局，不要自己写布局
擅于利用toolbar来作为APP的title栏。继承自baseactivity 会自动复写getToolbarTitle方法，直接把当前页面的title名字作为返回值返回即可显示

Toolbar默认开启了返回按键，返回按键默认已经加载了finish的点击事件，如果不需要显示返回键的页面（例如首页），则在afterview里面调用
getSupportActionBar().setDisplayHomeAsUpEnabled(false);方法即可隐藏返回按键

Toolbar的右边按钮的实现，先在menu文件夹下建立该activity对应的xml的menu文件，命名请遵从menu_(acitivity的名字)，新增一个item即可显示一个图标

例如     <item android:id="@+id/menu_main_test" android:title="测试"
        android:icon="@android:drawable/arrow_down_float"
        android:orderInCategory="100" app:showAsAction="always" />
icon如果不写直接显示文字， showasaction属性决定是否隐藏在右侧菜单栏


点击事件的实现：在activity里面复写onCreateOptionsMenu方法以及onOptionsItemSelected方法，具体使用参照安安租的mainactivity


======================================================================================================================================

（复杂版本的toolbar实现）
toolbar比actionbar高级的地方就是在于toolbar直接写在布局里面，有很强的自定义空间，在title里要实现复杂布局的时候需要自定义toolbar
自定义toolbar不需要走普通版TOOlbar流程，直接在你activity布局里面写上自定义的toobar，

例如：<android.support.v7.widget.Toolbar

    android:id="@id/diytoolbar"
    android:layout_width="match_parent"
    android:layout_height="60dp">

    <dev.xesam.android.study.lollipop.v7.toolbar.SlidingTabLayout
        android:id="@+id/v7_sliding_tabs"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
</android.support.v7.widget.Toolbar>


然后自己在activity处理自己的 toolbar


======================================================================================================================================
用snackbar代替toast 

google新控件是用来代替toast的，snackbar不仅可以提示信息，同时可以响应点击事件，默认单例，在baseactivity和basefragment里面均提供了相应的方法，showsnackbar（）；

同时，以前在fragment显示toast，需要getactivity强转到baseactivity才能调用的方法，现在fragment里可以直接调用，baseactivity的方法同样在basefragment也提供
（因为两个类实现了一个写有公共方法的接口），同时为了在部分情况下需要使用toast，我们也保留了showtoast方法（但不推荐使用）


======================================================================================================================================
本着业务拆分的原则，在studio的Android视图下的java文件夹下面有customer和landlord两个文件夹，房客的业务全部放在customer文件夹下建立自己的package
开发，房东业务全部放在landlord文件夹下开发，公用的业务放在主package里开发
======================================================================================================================================

使用recyleview代替listview

recyleview在性能上和使用上超越了他的前辈listview，在新的项目中使用更好的更优质的控件是我们的追求，我已经封装好了一个支持下拉刷新上啦加载功能的
的recyleview，使用更加便捷，控件名称为PullRecyclerView,使用demo会在PMS奉上。

======================================================================================================================================
使用MdEditText代替Editext
MdEditText是经过MD设计语言的Editext除了具有editext的属性之外，可以做到更好的交互效果，使用方法跟Edittext类似
   设置hint颜色用app:met_textColorHint
 * 设置右上角提示颜色用app:met_floatingTextColor
 * 设置默认状态下的下划线颜色（默认透明）app:met_underlineColor
 * 设置焦点状态下的下划线颜色 app:met_primaryColor默认透明
 * app:met_clearButton可是实现右边清除按钮
PS：更新，MdEdittext已支持Background属性
======================================================================================================================================
如果一个页面里面没有加Scroolview的情况下导致键盘弹出遮挡了编辑的editext，请加入Scroolview布局后重试，即使在正常情况下这个页面不需要滑动
都能看到所有的VIEW
======================================================================================================================================
finish()方法可以直接在异步线程里面调用
======================================================================================================================================
列表页的背景默认用selector_normal_pressed.xml
======================================================================================================================================
使用RecyclerView 写Adapter的时候请继承BaseRecyclerViewAdapter，里面为你封装好了item的点击事件，并将viewhodler跟itemview的处理方法
分开做了封装，简洁明了
======================================================================================================================================
textsize跟间距DP全部使用deimes文件里面的常量  textsize以sp_开头有不同的大小可供选择  dp以dp_开头有不同的大小可供选择，严禁写死
======================================================================================================================================
.9png图片放入drawable文件夹下，如果放入mipmap中会找不到
-====================================================================================================================================== 
-如果需要将当前获取的AnanzuUserinfo对象set一个值并且永久保存到本地，请调用AnanzuUserinfo对象的save方法保存该对象， 
-注意：这样的写法是错误的： 
- app.getmUserinfo().setiCurrentStatus(Types.STATUS_LANDLORD); 
- app.getmUserinfo().save(); 
-因为app.getmUserinfo()这个方法每次都会生成一个新的对象，调用save方法刚刚set的值不会生效 
-正确的写法： 
-  AnanzuUserInfo userInfo = app.getmUserinfo(); 
-  userInfo.setiCurrentStatus(Types.STATUS_CUSTORM); 
-  userInfo.save(); 
-====================================================================================================================================== 
页面没有数据的时候在继承自baseactivity和basefragment的类里面直接调用showErrorPage()方法即可