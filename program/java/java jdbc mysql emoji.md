* <http://stackoverflow.com/questions/39694531/jdbc-mysql-store-emojis-without-utf8mb4-encoding>

* Base64压缩

````
String mysqlColumn = MimeUtility.encodeWord(emojiStr);
````

* Base64解压

````
String emojiStr = MimeUtility.decodeWord(mysqlColumn);
````