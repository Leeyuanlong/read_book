# Map操作

- 查询
```java
Map<String, String> entries = redisTemplate.<String, String>opsForHash().entries(hashKey);
String orderStatus = entries.get(objectKey);
```

- 插入Map类型
```java
String hashKey = KeyGen.buildOrderKey(orderNo);
Map<String, String> keyValues = new HashMap<String, String>();
keyValues.put(map.key1, value1);
keyValues.put(map.key2, value2);
keyValues.put(map.key3, value3);
redisTemplate.opsForHash().putAll(hashKey, keyValues);
```

- 插入Object类型