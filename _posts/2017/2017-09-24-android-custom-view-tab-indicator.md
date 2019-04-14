---
layout: post
title: Android 自定义 View：写一个 Tab 选择器很简单
categories: Android
description: 学霸帮富二代作弊，一念之间改变人生轨迹
keywords: 电影 阶级差异 一念之间
---

实践出真知啊！还是得使用场景学习的更快。

# 1.写一个 Tab 选择器很简单


![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1555235900869&di=0191cced7389c31f8355da16a6b80169&imgtype=0&src=http%3A%2F%2Fimg.sucaihuo.com%2Fjquery%2F42%2F4283%2Fbig.jpg)


- 自定义 View 继承 ``LinearLayout``

- 内部持有一个 ``TextView`` 列表，设置权重为 TextView 的个数

- 在 ``onLayout()`` 方法中获取总宽度，除以 TextView 的个数，就是每个  Tab 的宽度

- 然后重写 ``dispatchDraw()``，在里面绘制一条线，线的偏移量 ``mTranslationX`` 等于前面的 Tab 个数乘以宽度



```

@Override
protected void onLayout(boolean changed, int l, int t, int r, int b) {
	super.onLayout(changed, l, t, r, b);
	mTabWidth = getMeasuredWidth() / mTabCount;
}

private void smoothScrollToPosition(final int position) {
	mCurrentPosition = position;

	if (isPlayingAnimation || mTitleTextViews == null ||
			position >= mTitleTextViews.length || mTitleTextViews[position] == null) {
		return;
	}

	float targetPositionX = position * mTabWidth;
	Logger.d("zsx", "targetPositionX: " + targetPositionX + ", position: " + position);
	ValueAnimator valueAnimator = ValueAnimator.ofFloat(mTranslationX, targetPositionX);
	valueAnimator.setDuration(getAnimationDuration());
	valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
		@Override
		public void onAnimationUpdate(ValueAnimator valueAnimator) {
			mTranslationX = (float) valueAnimator.getAnimatedValue();
			invalidate();
		}
	});
	valueAnimator.addListener(new AnimatorListenerAdapter() {
		@Override
		public void onAnimationEnd(Animator animation) {
			isPlayingAnimation = false;
			resetTitlesColor(position);
		}

		@Override
		public void onAnimationCancel(Animator animator) {
			isPlayingAnimation = false;
		}

		@Override
		public void onAnimationStart(Animator animation) {
			isPlayingAnimation = true;
		}
	});
	valueAnimator.start();
}

@Override
protected void dispatchDraw(Canvas canvas) {
	super.dispatchDraw(canvas);
	if (!hideIndicator) {
		canvas.save();
		float startX = mTranslationX + (mTabWidth - mIndicatorWidth) / 2;
		canvas.drawLine(startX, getHeight() - mIndicatorMarginToBottom - mIndicatorHeight /
				2, startX + mIndicatorWidth, getHeight() - mIndicatorMarginToBottom -
				mIndicatorHeight / 2, mIndicatorPaint);
		canvas.restore();
	}
}
```



# 2.``onDraw()`` 和 ``dispatchDraw()`` 的区别

>https://blog.csdn.net/scorplopan/article/details/6302827


- 绘制 View本身的内容，通过调用 ``View.onDraw(canvas)`` 函数实现

- 绘制自己的孩子通过 ``dispatchDraw(canvas)``实现

 

	View组件的绘制会调用draw(Canvas canvas)方法，draw过程中主要是先画Drawable背景，对 drawable调用setBounds()然后是draw(Canvas c)方法.有点注意的是背景drawable的实际大小会影响view组件的大小，drawable的实际大小通过getIntrinsicWidth()和getIntrinsicHeight()获取，当背景比较大时view组件大小等于背景drawable的大小

	 画完背景后，draw过程会调用onDraw(Canvas canvas)方法，然后就是dispatchDraw(Canvas canvas)方法, dispatchDraw() 主要是分发给子组件进行绘制，我们通常定制组件的时候重写的是onDraw()方法。



值得注意的是ViewGroup容器组件的绘制，当它没有背景时直接调用的是dispatchDraw()方法, 而绕过了draw()方法，当它有背景的时候就调用draw()方法，而** draw()方法里包含了dispatchDraw()方法的调用**。



因此**要在 ViewGroup 上绘制东西的时候往往重写的是 ``dispatchDraw()`` 方法而不是 ``onDraw()`` 方法**。或者自定制一个Drawable，重写它的draw(Canvas c)和 getIntrinsicWidth(), getIntrinsicHeight()方法，然后设为背景。



**小结：**



- 绘制顺序：先绘制 Drawable 背景（``setBounds()`` -> ``draw(Canvas c)``），然后 ``onDraw()``，然后 ``dispatchDraw()``

- 自定义 ViewGroup 时，没有背景时会直接调用 ``dispatchDraw()``

