软键盘 InputMethodManager

[toc]

## 1、软键盘的弹出

```kotlin
val manager = getSystemService(Context.INPUT_METHOD_SERVICE) 
				as InputMethodManager
manager.showSoftInput(searchPlaceEdit, 0)
```

说明：

- `showSoftInput(arg1, arg2)`
  - arg1，要求是可以且已经获取了焦点的EditText
    - EditText默认是可获取焦点的，所以此条件一般都可以满足。
    - 如果不满足，可以通过`view.setFocusable(true)`将其设置为可获取焦点的View。
  - arg2，提供一些额外的操作标记，可以取0或者SHOW_IMPLICIT
    - 0表示什么含义没有说明（**一般情况都是用0**）
    - SHOW_IMPLICIT表示本次显示软键盘的请求不是来自用户的直接请求，而是隐式的请求。

## 2、软键盘的隐藏

```kotlin
val manager = getSystemService(Context.INPUT_METHOD_SERVICE) 
				as InputMethodManager
manager.hideSoftInputFromWindow(drawerView.windowToken, 
                                InputMethodManager.HIDE_NOT_ALWAYS)
```

说明：

- `hideSoftInputFromWindow(arg1, arg2)`

  - arg1，指定一个View的windowToken，直接指定view调用windowToken即可

    - 官方文档中，应当是之前请求显示软键盘的View的windowToken；
    - 实际情况是，用任意一个当前布局中的已经加载的View的windowToken都可以隐藏软键盘，哪怕这个View被设置为INVISIBLE或GONE；

  - arg2，提供了一些额外的操作标记

    - 取0，无特别说明；

    - 取HIDE_IMPLICIT_ONLY，表示当前的软键盘应当只在其不是被用户显式的显示的时候才隐藏；

      => **说明：当显示键盘时，是使用SHOW_IMPLICT时，才能使用HIDE_IMPLICIT_ONLY进行隐藏**。

    - 还有一个HIDE_NOT_ALWAYS标记，它同样在文档中被遗忘了