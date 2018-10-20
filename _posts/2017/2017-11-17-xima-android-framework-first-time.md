---
layout: post
title: AndroidFramework 之初识输入事件传递机制
categories: Android
description: 路线指导
keywords: Android, Framework, 源码
---

XimaAndroidFramework 小组第一次分享交流（鹏飞 carry 全场）。

# 鹏飞分享的学习链接

- [Android系统Audio框架介绍](http://blog.csdn.net/yangwen123/article/details/39502689)
- https://source.android.com/devices/graphics/
- [Choreographer 这篇介绍的不错](http://www.jianshu.com/p/996bca12eb1d)
- [Android 显示系统源码分析](http://blog.csdn.net/yangwen123/article/category/1647761)
- [android surfaceflinger研究----显示系统](http://www.cnblogs.com/armlinux/archive/2012/02/29/2396756.html)
- [Android 6.0 VSync 信号如何到用户进程](http://blog.csdn.net/kc58236582/article/details/52892384)
- http://blog.csdn.net/kc58236582/article/category/6436488

# Android 输入事件传递机制

![](http://oqg4nua5z.bkt.clouddn.com/android/internal/android_input_system.png)

**学习资源链接：**

1. [http://newandroidbook.com/Book/Input.html?r](http://newandroidbook.com/Book/Input.html?r)
2. [http://newandroidbook.com/files/AndroidInput.pdf](http://newandroidbook.com/files/AndroidInput.pdf)
3. [https://seasonofcode.com/posts/internal-input-event-handling-in-the-linux-kernel-and-the-android-userspace.html](https://seasonofcode.com/posts/internal-input-event-handling-in-the-linux-kernel-and-the-android-userspace.html)

设备硬件，内核s，信号

> Server 进程

InputManagerService 

WMS  堆栈，Z 轴层级


> 应用进程

InputChannel

Click Touch

事件分发 *

ViewRootImpl -> DecorView -> DispatchEvent


adb shell getevent -t 检测事件

# IMS

三个处理器

native 层

1. event hub: 拿设备事件 01（增加事件、取消事件）  epoll, inotify
2. input read:  加工，策略
3. input dispatch：


# WMS

只有 View 的概念，层级

Binder, 

WindowToken ，父 window 子 window 共用一个 token 

应用层级：2000


Session.java add() 方法

InputChannel.java  Socket 发事件的

Choreographer

Surface, SurfaceFling

PhoneWindow	完成标题栏这些，


Dialog 和 Activity 只是公用一个 token


- KeyEvent 按焦点分发；
- MoveEvent 触摸事件按 ? 分发。

ViewGroup.dispatchTouchEvent


Click 在 touch 之后， View 里


WMS AMS PMS 在 system-server 中启动，在 ServiceManger 中注册

ActivityManagerService
``ActivityThread.mAllApplications``

```
final ArrayList<Application> mAllApplications = new ArrayList<Application>();
```

为了动态加载，loadedApk 进来就保存到这了。

```
public class Application extends ContextWrapper {...}
```

**我的插件化理解的还是皮毛的皮毛啊，加一吧！**


## WMS 学习指导代码

鹏飞分享的 WMS 学习指导代码:

```
package understanding.wms.samplewindow;
......
public class SampleWindow {
 
    public static void main(String[] args) {
        try {
             //SampleWindow.Run()是这个程序的主入口
            new SampleWindow().Run();
        } catch (Exception e) {
           e.printStackTrace();
        }
    }
    //IWindowSession 是客户端向WMS请求窗口操作的中间代理，并且是进程唯一的
    IWindowSession mSession = null;
    //InputChannel 是窗口接收用户输入事件的管道。在第5章中将对其进行详细的探讨
    InputChannel mInputChannel = new InputChannel();
 
    // 下面的三个Rect保存了窗口的布局结果。其中mFrame表示了窗口在屏幕上的位置与尺寸
    // 在4.4中将详细介绍它们的作用以及计算原理
    Rect mInsets = new Rect();
    Rect mFrame = new Rect();
    Rect mVisibleInsets = new Rect();
 
    Configuration mConfig = new Configuration();
    // 窗口的Surface，在此Surface上进行的绘制都将在此窗口上显示出来
    Surface mSurface = new Surface();
    // 用于在窗口上进行绘图的画刷
    Paint mPaint = new Paint();
    // 添加窗口所需的令牌，在4.2节将会对其进行介绍
    IBinder mToken = new Binder();
 
    // 一个窗口对象，本例演示了如何将此窗口添加到WMS中，并在其上进行绘制操作
    MyWindow mWindow = new MyWindow();
 
    //WindowManager.LayoutParams定义了窗口的布局属性，包括位置、尺寸以及窗口类型等
    LayoutParams mLp = new LayoutParams();
 
    Choreographer mChoreographer = null;
    //InputHandler 用于从InputChannel接收按键事件做出响应
    InputHandler mInputHandler = null;
 
    boolean mContinueAnime = true;
 
    public void Run() throws Exception{
       Looper.prepare();
       // 获取WMS服务
       IWindowManager wms = IWindowManager.Stub.asInterface(ServiceManager.getService(Context.WINDOW_SERVICE));
 
       // 通过WindowManagerGlobal获取进程唯一的IWindowSession实例。它将用于向WMS
       // 发送请求。注意这个函数在较早的Android版本（如4.1）位于ViewRootImpl类中
       mSession= WindowManagerGlobal.getWindowSession(Looper.myLooper());
 
       // 获取屏幕分辨率
       IDisplayManager dm = IDisplayManager.Stub.asInterface(ServiceManager.getService(Context.DISPLAY_SERVICE));
       DisplayInfo di = dm.getDisplayInfo(Display.DEFAULT_DISPLAY);
       Point scrnSize = new Point(di.appWidth, di.appHeight);
       // 初始化WindowManager.LayoutParams
       initLayoutParams(scrnSize);
 
       // 将新窗口添加到WMS
       installWindow(wms);
 
       // 初始化Choreographer的实例，此实例为线程唯一。这个类的用法与Handler
       // 类似，不过它总是在VSYC同步时回调，所以比Handler更适合做动画的循环器[1]
       mChoreographer= Choreographer.getInstance();
 
       // 开始处理第一帧的动画
       scheduleNextFrame();
 
       // 当前线程陷入消息循环，直到Looper.quit()
       Looper.loop();
 
       // 标记不要继续绘制动画帧
       mContinueAnime= false;
 
       // 卸载当前Window
       uninstallWindow(wms);
    }
 
    public void initLayoutParams(Point screenSize) {
       // 标记即将安装的窗口类型为SYSTEM_ALERT，这将使得窗口的ZOrder顺序比较靠前
       mLp.type = LayoutParams.TYPE_SYSTEM_ALERT;
       mLp.setTitle("SampleWindow");
       // 设定窗口的左上角坐标以及高度和宽度
       mLp.gravity = Gravity.LEFT | Gravity.TOP;
       mLp.x = screenSize.x / 4;
       mLp.y = screenSize.y / 4;
       mLp.width = screenSize.x / 2;
       mLp.height = screenSize.y / 2;
       // 和输入事件相关的Flag，希望当输入事件发生在此窗口之外时，其他窗口也可以接受输入事件
       mLp.flags = mLp.flags | WindowManager.LayoutParams.FLAG_NOT_TOUCH_MODAL;
    }
 
    public void installWindow(IWindowManager wms) throws Exception {
       // 首先向WMS声明一个Token，任何一个Window都需要隶属与一个特定类型的Token
       wms.addWindowToken(mToken,WindowManager.LayoutParams.TYPE_SYSTEM_ALERT);
       // 设置窗口所隶属的Token
       mLp.token = mToken;
       // 通过IWindowSession将窗口安装进WMS，注意，此时仅仅是安装到WMS，本例的Window目前仍然没有有效的Surface。不过，经过这个调用后，mInputChannel已经可以用来接受输入事件了
       mSession.add(mWindow,0, mLp, View.VISIBLE, mInsets, mInputChannel);
       // 通过IWindowSession要求WMS对本窗口进行重新布局，经过这个操作后，WMS将会为窗口创建一块用于绘制的Surface并保存在参数mSurface中。同时，这个Surface被WMS放置在LayoutParams所指定的位置上 
       mSession.relayout(mWindow,0, mLp, mLp.width, mLp.height, View.VISIBLE,0, mFrame, mInsets, mVisibleInsets, mConfig, mSurface);
       if(!mSurface.isValid()) {
            throw new RuntimeException("Failed creating Surface.");
       }
       // 基于WMS返回的InputChannel创建一个Handler，用于监听输入事件
       //mInputHandler一旦被创建，就已经在监听输入事件了
       mInputHandler= new InputHandler(mInputChannel, Looper.myLooper());
    }
 
    public void uninstallWindow(IWindowManager wms) throws Exception {
       // 从WMS处卸载窗口
       mSession.remove(mWindow);
       // 从WMS处移除之前添加的Token
       wms.removeWindowToken(mToken);
    }
 
    public void scheduleNextFrame() {
       // 要求在显示系统刷新下一帧时回调mFrameRender，注意，只回调一次
       mChoreographer.postCallback(Choreographer.CALLBACK_ANIMATION, mFrameRender, null);
    }
 
    // 这个Runnable对象用以在窗口上描绘一帧
    public Runnable mFrameRender = new Runnable() {
       @Override
        public void run() {
            try{
               // 获取当期时间戳
               long time = mChoreographer.getFrameTime() % 1000;
 
               // 绘图
               if (mSurface.isValid()) {
                   Canvas canvas = mSurface.lockCanvas(null);
                   canvas.drawColor(Color.DKGRAY);
                   canvas.drawRect(2 * mLp.width * time / 1000 - mLp.width, 0, 2 *mLp.width * time / 1000, mLp.height,mPaint);
                   mSurface.unlockCanvasAndPost(canvas);
                   mSession.finishDrawing(mWindow);
               }
 
            if(mContinueAnime)
               scheduleNextFrame();
           } catch (Exception e) {
               e.printStackTrace();
           }
        }
    };
 
    // 定义一个类继承InputEventReceiver，用以在其onInputEvent()函数中接收窗口的输入事件
    class InputHandler extends InputEventReceiver {
       Looper mLooper = null;
       public InputHandler(InputChannel inputChannel, Looper looper) {
            super(inputChannel,looper);
            mLooper= looper;
       }
       @Override
        public void onInputEvent(InputEvent event) {
            if(event instanceof MotionEvent) {
               MotionEvent me = (MotionEvent)event;
                if (me.getAction() ==MotionEvent.ACTION_UP) {
                   // 退出程序
                   mLooper.quit();
               }
           }
            super.onInputEvent(event);
        }
    }
 
    // 实现一个继承自IWindow.Stub的类MyWindow。
    class MyWindow extends IWindow.Stub {
        // 保持默认的实现即可
    }
}

```