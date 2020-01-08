### 5.Zset

- 应用场景举例

	- 可以用于大型在线游戏的积分排行榜
	- 可以用于构建索引数据

- 命令

	- ZADD key score1 mem1 score2 mem2 score3 mem3
	- ZCOUNT key min max--指定分数范围的元素个数
	- ZCARD key--元素的数量
	- ZSCORE key mem--获取元素的分数
	- ZINCRBY key increment mem--增加元素的分数
	- ZREVRANGE key start stop [withscopes]--获取排名在某个范围的元素列表（从大到小）
	- ZRANGE key start stop [withscores]--获得排名在某个范围的元素列表（从小到大）
	- ZRANGEBYSCORE key min max [withscores] [limit offset count]
	- ZREVRANGEBYSCORE key max min [withscores] [limit offset count]
	- ZRANK key mem--获取元素排名
	- ZREVRANK key mem--获取元素逆序排名（按照分数从大到小排序）
	- ZREM key mem1 mem2 mem3 ...--删除元素
	- ZREMRANGEBYRANK key start stop--按照排名范围删除元素
	- ZREMRANGEBYSCORE key start stop--按照分数范围删除

*XMind: ZEN - Trial Version*