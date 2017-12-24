## Android-视图层级实时分析
>注：
 1.笔者分析代码使用的API版本为API 26 (Android 8.0)<br>
 2.笔者使用的模拟器为Android 8.0<br>

<font size = '4'></font>
<font size = '4'>首先，笔者先来一个简单的Demo实例。我们使用Android Studio新建一个Empty Android工程，跑一下程序，界面如下图所示：</font><br>

![demo.png](https://github.com/tuke0919/CompatStatusBar/blob/master/shotscreen/demo.png)<br>

<font size = '4'>接下来，我们要对视图层级进行分析,**Android Studio**也有自带的视图分析工具 **Layout Inspector（布局检查器）**,打开方式：Tools->Android->Layout Inspector .</font><br>

## 正文
<font size = '4'>从根视图开始分析视图层级，如下图所示：</font><br>
![decorview.png](https://github.com/tuke0919/CompatStatusBar/blob/master/shotscreen/decorview.png)<br>

* <font size = '4'>我们可以看到视图的根布局是DecorView，宽度和高度为模拟器屏幕宽高,mTop,mLeft,mRight,mBottom是相对于父布局的上下左右</font><br>
* <font size = '4'>DecorView中包含3个子View:LinearLayout、View(android.R.id.navigationBarBackground)、View(android.R.id.statusBarBackground)</font><br>
* <font size = '4'>DecorView是继承FrameLayout帧布局，所以三个子View从上到下依次是 状态栏--导航栏(有的手机没有)--LinearLayout</font><br>

1. DecorView的第一个子View状态栏  
![statusbar.png](https://github.com/tuke0919/CompatStatusBar/blob/master/shotscreen/statusbar.png)  

* 从图中可以看出，状态栏的高度是63px，状态栏的id是android.R.id.statusBarBackground













