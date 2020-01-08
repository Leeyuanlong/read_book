### 4.Set

- sadd key mem1 mem2 ...
- scard key--获取集合中元素的个数
- sismember key mem
- smembers key--获得集合中所有元素
- spop key--集合中随机弹出一个元素
- SREM key mem1 mem2 ...--删除元素
- SRANDOMMEMBER key [count]--随机获得元素，但是不删除元素
- SMOVE source destination member
- SDIFF key1 key2 key3--将key1与key2比较的结果与key3比较
- SDIFFSTORE destination key1 key2
- SINTER key1 key2...
- SINTERSTORE destination key1 key2 ...
- SUNION key1 key2 ...
- SUNION destination key1 key2 ...

*XMind: ZEN - Trial Version*