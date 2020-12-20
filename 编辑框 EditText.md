 编辑框 EditText

[toc]

## 1、不能自动获取焦点，无法弹出软键盘

```kotlin
editText.isFocusable = true
editText.isFocusableInTouchMode = true
editText.requestFocus()
```

说明：

- `isFocusable`，设置为可以获取焦点
- `isFocusableInTouchMode`，设置为触碰时获取焦点
- `requestFocus()`，请求焦点

## 2、光标位置

```kotlin
editText.setSelection(str.length())
```

说明：在填充`editText`的字符串末尾，设置为光标的位置

## 3、其他

[EditText 光标颜色和宽度设置](https://blog.csdn.net/qq_34115898/article/details/91039847)