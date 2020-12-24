DocumentFile 相关操作

### 一、获取DocumentFile

```kotlin
/** uri 来自于系统授权目录在数据库中的值 **/
// 获取目录
val documentTree = DocumentFile.fromTreeUri(context, uri)
// 获取文件
val document = DocumentFile.fromSingleUri(context, uri)

// 获取目录下的文件列表
val childDocuments = documentTree?.listFiles()?.toMutableList()
```



### 二、获取文件名称和文件大小

```kotlin
// 全职法师.txt
val documentName = document.name

val documentSize = document.length()

val documentLastModified = document.lastModified()

val documentType = document.type 
```



### 三、格式化数据

**document.name**

```kotlin
var name = document.name
val lastPointIndex = name!!.lastIndexOf(".")
name = name.substring(0, lastPointIndex)
```

**document.type** 

```kotlin
// 数据内容：text/plain
when (document.type) {
    "text/plain" -> { …… }
}
```



**document.lastModified()**

```kotlin
fun getFormatFileLastModified(time: Long): String {
    return if (android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.O) {
        val instant = Instant.ofEpochMilli(time)
        val date = LocalDateTime.ofInstant(instant, ZoneId.systemDefault())
        val format = DateTimeFormatter.ofPattern("yyyy/MM/dd HH:mm")
        format.format(date).toString()
    } else {
        // 这部分没用放在android O以下测试
        val date = Date(time)
        val format = SimpleDateFormat("yyyy/MM/dd HH:mm")
        format.format(date)
    }
}
```

**document.length()**

```kotlin
fun getFormatFileSize(length: Long): String {
    LogUtil.d("MainTest", "文件大小为：$length")
    val arr = arrayOf("Bytes", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
    var index = 0
    var value = length.toDouble()
    while (value > 1024) {
        value /= 1024
        index++
    }
    var size = length / 1024.0.pow(index.toDouble())
    val sizeStr = DecimalFormat("#.0").format(value)
    return "$sizeStr ${arr[index]}"

}
```

### 四、根据DocumentFile.uri，获得InputStream

```kotlin
fun getInputStream(documentFile: DocumentFile) = context.contentResolver.openInputStream(documentFile.uri)
```

### 五、根据Uri，遍历目录

```kotlin
fun loadDirectory(directoryUri: Uri) {
        val documentsTree = DocumentFile.fromTreeUri(getApplication(), directoryUri) ?: return
        val childDocuments = documentsTree.listFiles().toCachingList()

        // It's much nicer when the documents are sorted by something, so we'll sort the documents
        // we got by name. Unfortunate there may be quite a few documents, and sorting can take
        // some time, so we'll take advantage of coroutines to take this work off the main thread.
        viewModelScope.launch {
            val sortedDocuments = withContext(Dispatchers.IO) {
                childDocuments.toMutableList().apply {
                    sortBy { it.name }
                }
            }
            _documents.postValue(sortedDocuments)
        }
    }
```



**其他**：

- [Android 文件绝对路径和Content开头的Uri互相转换](https://www.jianshu.com/p/02fa61d8dbf5)

- [uri,file,path互相转化](https://blog.csdn.net/d276031034/article/details/52781375)

  