# Map操作

- 查询map
```java
Map<String, String> entries = redisTemplate.<String, String>opsForHash().entries(hashKey);
String orderStatus = entries.get(objectKey);
```
