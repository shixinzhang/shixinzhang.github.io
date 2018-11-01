---
layout: post
title: Android 事件分发小记
categories: Android
description: 记录安卓事件分发相关的内容
keywords: 安卓 事件分发
---


整理一下安卓事件分发相关的内容，方便复习。

不错的文章：


- http://www.gcssloop.com/customview/dispatch-touchevent-theory
- https://www.diycode.cc/topics/352
- https://blog.csdn.net/carson_ho/article/details/54136311



# 执行顺序

**先后执行顺序：**


1. ``onInterceptTouchEvent()`` 决定要不要拦截这个事件
2. ``OnTouchListener.onTouch()`` 外层设置的处理拦截，决定要不要分发给这个布局
3. ``onTouchEvent()`` 根据事件做相应的处理，返回结果表示当前的处理是否结束，没结束就调用父布局的处理
4. ``OnClickListener.onClick()`` 外层设置的点击处理逻辑

![](http://oqg4nua5z.bkt.clouddn.com/blog/0-event-dispatch.png)

> ``onLongClickListener`` 在 ``onClickListener`` 前面，它的返回值决定要不要调用 ``onClickListener`` 。

# dispatchTouchEvent()

ViewGroup 和 View 对这个方法的实现有些不同。

**ViewGroup 的实现是决定把事件分发给当前布局还是子布局；而 View 的实现则是决定把点击事件分发给 ``OnTouchListener`` 还是 ``onTouchEvent()`` 还是 ``onLongClickListener`` ``onClickListener``。**

``ViewGroup.dispatchTouchEvent()`` 实现伪代码：

```

public boolean dispatchTouchEvent(MotionEvent ev) {

    boolean result = false;            // 默认状态为没有消费过
    
    if (!onInterceptTouchEvent(ev)) {  // 如果没有拦截交给子View
        result = child.dispatchTouchEvent(ev);
    }

    if (!result) {                      // 如果事件没有被消费,询问自身onTouchEvent
        result = onTouchEvent(ev);
    }

    return result;

}

```



``View.dispatchTouchEvent()`` 实现伪代码：


```

public boolean dispatchTouchEvent(MotionEvent event) {

  if (mOnTouchListener.onTouch(this, event)) {        //返回 true 表示不调用默认的 onTouchEvent
      return true;

  } else if (onTouchEvent(event)) {
      return true;
  }

  return false;

}

```



# ViewGroup.onInterceptTouchEvent()



> ViewGroup 才有的拦截方法



点击事件最先触发的方法 ``ViewGroup.onInterceptTouchEvent()`` ，这个方法决定是由你处理点击事件，还是分发给孩子。



- 如果  ``ViewGroup.onInterceptTouchEvent()`` 返回 true，表示当前父布局拦截了，就会触发你的父布局的 ``onTouchEvent()`` ，为了接收接下来的事件，你的  ``onTouchEvent()`` 需要返回 true。



- 如果  ``ViewGroup.onInterceptTouchEvent()`` 返回 false，表示不拦截，会调用子布局的 ``onIntercepTouchEvent()``

 - 注意！！！**如果在 down 事件时都返回 false，会导致接下来的事件都不会传到父布局，直接传到子布局的拦截方法** 

 - 因此，一般在 down 事件不返回 false，以便可以接收接下来的事件





# View.onTouchListener

在点击事件被分发给当前布局之前调用，用于最后一次拦截。

返回 true 表示被处理了，不调用 OnClickListener 的事件。

>如果给布局设置 onTouchListener 有没调用 ``performClick()`` 的警告，就给这个布局复写一下 ``performClick()`` 方法


# View.onTouchEvent()


 一旦  ``onTouchEvent()`` 返回 true ，除了 `` ACTION_CANCEL`` 事件，别的事件不会再触当前 布局的 ``onInterceptTouchEvent()``，直接触发你的  ``onTouchEvent()`` 

- ``onTouchEvent()`` 返回 true，表示当前布局消耗了事件，不会往下传
- 如果 你的  ``onTouchEvent()`` 返回 false，则会去寻找父布局来处理这个点击事件，调用父布局的 ``onTouchEvent()``

只有布局是“可点击的”   ``onTouchEvent()`` 才会往下执行，调用 ``onClickListener`` 和 ``onLongClickListener`` 等代码，这个可点击的定义是：

```
final boolean clickable = ((viewFlags & CLICKABLE) == CLICKABLE
        || (viewFlags & LONG_CLICKABLE) == LONG_CLICKABLE)
        || (viewFlags & CONTEXT_CLICKABLE) == CONTEXT_CLICKABLE;
```

只要给 View 注册了 ``onClickListener、onLongClickListener、OnContextClickListener`` 其中的任何一个监听器或者设置了 ``android:clickable="true"`` 就代表这个 View 是可点击的。

> 其中 ``CONTEXT_CLICKABLE`` 和 ``onContextClickListener``，目前的说法是指鼠标右键，不太确定。


```
public boolean onTouchEvent(MotionEvent event) {
    //...
    if (mTouchDelegate != null) {
        if (mTouchDelegate.onTouchEvent(event)) {
            return true;
        }
    }

    if (clickable || (viewFlags & TOOLTIP) == TOOLTIP) {
        //...
        return true;
    }

    return false;
}
```

**从上面的代码可以看到，只要 View 是 ``clickable`` 的，就会消费事件！**


``View.onTouchEvent()`` 中还有一个很有用的判断：

```
public boolean onTouchEvent(MotionEvent event) {
    //...
    if (mTouchDelegate != null) {
        if (mTouchDelegate.onTouchEvent(event)) {
            return true;
        }
    }
    //...
}
```

这个 ``TouchDelegate``，当我们想要给一个布局比自己本山区域更大或者更小的点击区域时，可以使用它。

使用方法如下所示：

```
public static void expandClickArea(final Context context, final View view,
      final int left, final int top, final int right, final int bottom) {
   view.post(new Runnable() {

      @Override
      public void run() {
         if (context == null) {
            return;
         }
         Rect delegateArea = new Rect();
         View delegate = view;
         delegate.getHitRect(delegateArea);
         delegateArea.top -= BaseUtil.dp2px(context, top);
         delegateArea.bottom += BaseUtil.dp2px(context, bottom);
         delegateArea.right += BaseUtil.dp2px(context, right);
         delegateArea.left -= BaseUtil.dp2px(context, left);
         TouchDelegate expandedArea = new TouchDelegate(delegateArea,
               delegate);
         if (View.class.isInstance(delegate.getParent())) {
            ((View) delegate.getParent())
                  .setTouchDelegate(expandedArea);
         }
      }
   });
}
```


# View.onLongClickListener

**OnClick 和 OnLongClick 的具体调用位置在 onTouchEvent 中。**

在按下（``MotionEvent.ACTION_DOWN``） 时启动了一个调用 ``onLongClickListener.onLongClick()`` 的 runnable，执行时间默认是 500ms。在 ``MotionEvent.ACTION_MOVE`` ``MotionEvent.ACTION_CANCEL`` 和 ``MotionEvent.UP`` 里移除了这个 runnable。

> 判断是一个点击时间的时间间隔是：100ms
> 判断是否为长按的时间：500ms


----------------------------



事件收集到后发给 Activity ，Activity 发给 PhoneWindow，然后按照上图往下传；如果到底层还没人能处理，就会反过来往上传。

> 非常经典的责任链模式， 如果我能处理就拦截下来自己干，如果自己不能处理或者不确定就交给责任链中下一对象。


--------------

# 摘录部分未验证的

摘自：https://www.diycode.cc/topics/352

1.ViewGroup 和 ChildView 同时注册了事件监听器(onClick等)，哪个会执行?

答：事件优先给 ChildView，会被 ChildView消费掉，ViewGroup 不会响应。


> 应该说的是父布局没有重写 拦截方法的情况下吧。

2.安卓为了保证所有的事件都是被一个 View 消费的，对第一次的事件( ACTION_DOWN )进行了特殊判断

**View 只有消费了 ACTION_DOWN 事件，才能接收到后续的事件(可点击控件会默认消费所有事件)，并且会将后续所有事件传递过来，不会再传递给其他 View，除非上层 View 进行了拦截。**

如果上层 View 拦截了当前正在处理的事件，会收到一个 ``ACTION_CANCEL``，表示当前事件已经结束，后续事件不会再传递过来。



