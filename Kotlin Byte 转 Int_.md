Kotlin Byte 转 Int

基本逻辑：

将数据用toInt进行补位，再使用（and/&）进行位运算

Java：

```java
ByteArray value = ……
Integer valueInt = value[2] & 0xFF
```

Kotlin

```kotlin
val value = byteArrayOf(0xE5.toByte(), 0x22, 0xA5.toByte(), 0x03, 0x00)
val valueInt = value[2].toInt() and 0xff
```

