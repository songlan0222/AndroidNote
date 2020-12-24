获取txt类型文件的编码方式

| Java中的编码字符串 | Text编码                                                     | 字节标志                                                     |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| GBK                | ANSI                                                         | 无格式定义                                                   |
| UTF-8              | UTF-8包含两种规格：UTF-8UTF-8-BOM                            | 需判断前三个字节：前三个字节为：0xE59B9E前三个字节为：0xEFBBBF |
| UTF-16             | UTF-16                                                       | 前两个字节为：0xFEFF                                         |
| UNICODE            | Unicode包含两种规格：1、UCS2 Little Endian2、UCS2 Big Endian | 前两个字节为：0xFFFE                                         |

Kotlin

```kotlin
fun getFileCharsetName(fileName: String): String? {
        val inputStream: InputStream = FileInputStream(fileName)
        val head = ByteArray(3)
        inputStream.read(head)
        var charsetName = "GBK" //或GB2312，即ANSI
        if (head[0].toInt() and 0xff == 0xFF 
           && head[1].toInt() and 0xff == 0xFE
        ) // 0xFFFE
            charsetName = "UTF-16"
        else if (head[0].toInt() and 0xff == 0xFE &&
            head[1].toInt() and 0xff == 0xFF
        ) // 0xFEFF
			//包含两种编码格式：UCS2-Big-Endian和UCS2-Little-Endian
    		charsetName = "Unicode" 
        else if (head[0].toInt() and 0xff == 0xE5 &&
            head[1].toInt() and 0xff == 0x9B &&
            head[2].toInt() and 0xff == 0x9E
        ) // 0xE59B9E
            charsetName = "UTF-8" //UTF-8(不含BOM)
        else if (head[0].toInt() and 0xff == 0xEF &&
            head[1].toInt() and 0xff == 0xBB &&
            head[2].toInt() and 0xff == 0xBF
        )// 0xEFBBBF
            charsetName = "UTF-8" //UTF-8-BOM
        
    	// 输入流使用之后，记得关闭
    	inputStream.close()

        //System.out.println(code);
        return charsetName
    }
```

