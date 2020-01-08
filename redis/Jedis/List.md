# List操作

- 查询
```java
List<String> strings = redisTemplate.opsForList().range(key, 0, 1000);

```
![List操作](https://github.com/Leeyuanlong/pict_bank/raw/master/redis/Jedis/List_Operator.jpg)
