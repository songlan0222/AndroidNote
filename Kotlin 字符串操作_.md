Kotlin 字符串操作

### 一、空格处理

```kotlin
val str = " abc, edf, ghi, jkl "
// 去掉开头空格
val strTrimStart = str.trimStart()

// 去掉末尾空格
val strTrimEnd = str.trimEnd()

// 去掉两端空格
val strTrim = str.trim()

// 去掉全部空格
val strTrimAll = str.filter{ !it.isWhilteSpace() }
```



### 二、查找子串最后一次出现的位置

```kotlin
val str = "全职法师.txt"
val lastIndex = str.lastIndexOf(".")
```



### 三、字符串截取

```kotlin
val str = "全职法师.txt"
val subString = str.subString(0, lastIndex)
```

