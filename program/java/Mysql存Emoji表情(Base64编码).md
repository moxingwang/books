### Mysql的utf8编码为何存储不了Emoji表情？


### 让Mysql支持Emoji表情的多种方式

1. 修改Mysql的表面为utf8_mb4

2. 服务端使用Base64转换Emoji编码
    * 服务端对Emoji表情进行Base64压缩
    ````
    String mysqlColumn = MimeUtility.encodeWord(emojiStr);
    ````

    * 对数据存储的Base64编码后的字符串逆向解码

    ````
    String emojiStr = MimeUtility.decodeWord(mysqlColumn);
    ````

### 参考文章
* <http://blog.csdn.net/u012918303/article/details/44492315>