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

### 1. DecorView的第一个子View--状态栏  
![statusbar.png](https://github.com/tuke0919/CompatStatusBar/blob/master/shotscreen/statusbar.png)  

* 从图中可以看出，状态栏的高度是63px，作用是当状态栏背景，状态栏的id是android.R.id.statusBarBackground
### 2. DecorView的第二个子View--导航栏  
![navigationbar.png](https://github.com/tuke0919/CompatStatusBar/blob/master/shotscreen/navigation.png)  

* 从图中可以看出，导航栏的高度是126px，作用是当导航栏背景，导航栏的id是android.R.id.navigationBarBackground  
### 3. DecorView的第三个子View--LinearLayout  
  ![linearlayout.png](https://github.com/tuke0919/CompatStatusBar/blob/master/shotscreen/linearlayout.png)  
  * 可以看出，LinearLayout的区域是除去导航栏的区域，layout_bottomMargin=126，是把导航栏去除了。
  * LinearLayout包含两个子View：**ViewStub和FrameLayout**  
  * ViewStub显示的文字颜色为灰色，说明该View没有在当前视图上显示出来。 
ViewStub的属性信息，如下图所示：  
![viewstub](https://github.com/tuke0919/CompatStatusBar/blob/master/shotscreen/viewstub.png)  
* ViewStub的getVisibility属性值为 GONE (ViewStub处于隐藏状态)  
FrameLayout的属性信息，如下图所示：  
![framelayout.png](https://github.com/tuke0919/CompatStatusBar/blob/master/shotscreen/framelayout.png)  
* FrameLayout的高度值为1731px，相比于父布局LinearLayout又少了63px。
* 从mTop属性可以知道，FrameLayout在布局时顶点坐标高度从63px开始，空出了状态栏的高度。

接下来，继续分析FrameLayout的子View，FrameLayout只有一个子View：**ActionBarOverlayLayout**。ActionBarOverlayLayout的宽度和高度与FrameLayout保持一致。  
ActionBarOverlayLayout有两个子View：**ContentFrameLayout和ActionBarContainer**  
ContentFrameLayout的视图属性，如下图所示：<br>
![contentframelayout.png](https://github.com/tuke0919/CompatStatusBar/blob/master/shotscreen/contentframelayout.png)
* ContentFrameLayout的高度相比于父布局减少了147px。
* 从layout_topMargin属性可以看出，这147px用于预留顶部边界，空出了标题栏的高度。
* ContentFrameLayout的mID属性值为content（android.R.id.content）
* ContentFrameLayout里的子View就是我们activity_main.xml中的视图了。  
![activity_main.png](https://github.com/tuke0919/CompatStatusBar/blob/master/shotscreen/activity_main.png)

ActionBarContainer的视图属性，如下图所示：  

![actionbar](https://github.com/tuke0919/CompatStatusBar/blob/master/shotscreen/action_bar_container.png)  

* ActionBarContainer从名字也可以看出这是一个标题栏容器。
* ActionBarContainer的子View中当前处于显示状态的只有一个Toolbar。










