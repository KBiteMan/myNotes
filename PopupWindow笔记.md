---
title: PopupWindow笔记
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

# 一、简要
## 1. PopupWindow与AlertDialog的区别
AlertDialog无法指定显示位置，只能显示在屏幕的正中间（可以通过WindowManger来设置显示位置）。PopupWindow可以任意设置显示位置。

## 2.相关函数

### 1.构造函数

|     |   说明  |
| :---: | :--- |
|PopupWindow(Context context)|在（0,0）位置创建一个空的，没有焦点的PopupWindow.|
|PopupWindow(Context context, AttributeSet attrs)|Create a new empty, non focusable popup window of dimension (0,0).|
|PopupWindow(Context context, AttributeSet attrs, int defStyleAttr)|Create a new empty, non focusable popup window of dimension (0,0).|
|PopupWindow(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes)|Create a new, empty, non focusable popup window of dimension (0,0).|
|PopupWindow()|在（0,0）位置创建一个空的，没有焦点的PopupWindow|
|PopupWindow(View contentView)|创建一个包含指定视图布局的，没有焦点的PopupWindow.|
|PopupWindow(int width, int height)|创建一个空的，没有焦点的，指定宽高的PopupWindow.|
|PopupWindow(View contentView, int width, int height)|创建一个包含指定视图布局的，没有焦点的，指定宽高的PopupWindow.|
|PopupWindow(View contentView, int width, int height, boolean focusable)|创建一个包含指定视图布局的，指定焦点的，指定宽高的PopupWindow.|

> 默认的PopupWindow是没有布局的，所以必须强制设置布局。

创建一个PopupWindow的正确方法是（可以使用其他构造函数）：
```java
View contentView = LayoutInflater.from(MainActivity.this).inflate(R.layout.popuplayout, null);  
PopupWindwo popWnd = new PopupWindow (context);  
popWnd.setContentView(contentView);  
popWnd.setWidth(ViewGroup.LayoutParams.WRAP_CONTENT);  
popWnd.setHeight(ViewGroup.LayoutParams.WRAP_CONTENT);  
```

### 2. 设置类函数

|  返回值   |   方法  |   说明  |
| :---: | :---: | :--- |
|void|	setAnimationStyle(int animationStyle)|设置弹框的动画.|
|void|	setAttachedInDecor(boolean enabled)|This will attach the popup window to the decor frame of the parent window to avoid overlaping with screen decorations like the navigation bar.|
|void|	setBackgroundDrawable(Drawable background)|Specifies the background drawable for this popup window.|
|void|	setClippingEnabled(boolean enabled)|Allows the popup window to extend beyond the bounds of the screen.|
|void|	setContentView(View contentView)|Change the popup's content.|
|void|	setElevation(float elevation)|Specifies the elevation for this popup window.|
|void|	setEnterTransition(Transition enterTransition)|Sets the enter transition to be used when the popup window is shown.|
|void|	setExitTransition(Transition exitTransition)|Sets the exit transition to be used when the popup window is dismissed.|
|void|	setFocusable(boolean focusable)|Changes the focusability of the popup window.|
|void|	setHeight(int height)|Sets the popup's requested height.|
|void|	setIgnoreCheekPress()|Set the flag on popup to ignore cheek press events; by default this flag is set to false which means the popup will not ignore cheek press dispatch events.|
|void|	setInputMethodMode(int mode)|Control how the popup operates with an input method: one of INPUT_METHOD_FROM_FOCUSABLE, INPUT_METHOD_NEEDED, or INPUT_METHOD_NOT_NEEDED.|
|void|	setOnDismissListener(PopupWindow.OnDismissListener onDismissListener)|Sets the listener to be called when the window is dismissed.|
|void|	setOutsideTouchable(boolean touchable)|Controls whether the pop-up will be informed of touch events outside of its window.|
|void|	setOverlapAnchor(boolean overlapAnchor)|Sets whether the popup window should overlap its anchor view when displayed as a drop-down.|
|void|	setSoftInputMode(int mode)|Sets the operating mode for the soft input area.|
|void|	setSplitTouchEnabled(boolean enabled)|Allows the popup window to split touches across other windows that also support split touch.|
|void	|setTouchInterceptor(View.OnTouchListener l)|Set a callback for all touch events being dispatched to the popup window.|
|void|	setTouchable(boolean touchable)|Changes the touchability of the popup window.|
|void|	setWidth(int width)|Sets the popup's requested width.|
|void|	setWindowLayoutMode(int widthSpec, int heightSpec)|This method was deprecated in API level 23. Use setWidth(int) and setHeight(int).|
|void|	setWindowLayoutType(int layoutType)|Set the layout type for this window.|

### 3. 显示函数

|  返回值   |   方法  |   说明  |
| :---: | :---: | :--- |
|void|showAsDropDown(View anchor)|在锚视图的左下角的一个弹出窗口。|
|void|showAsDropDown(View anchor, int xoff, int yoff, int gravity)| 相对于某个控件的位置（例如正中央Gravity.CENTER，下方Gravity.BOTTOM等），可以设置偏移或无偏移 |
|void|showAsDropDown(View anchor, int xoff, int yoff)|相对某个控件的位置，有偏移；xoff表示x轴的偏移，正值表示向左，负值表示向右；yoff表示相对y轴的偏移，正值是向下，负值是向.|
|void|showAtLocation(View parent, int gravity, int x, int y)|相对于父控件的位置（例如正中央Gravity.CENTER，下方Gravity.BOTTOM等），可以设置偏移或无偏移|
