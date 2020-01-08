# Jedis配置

## Spring Jedis配置
application-redis-context.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd"
       default-lazy-init="false">

    <bean id="poolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <property name="maxTotal" value="${redis.maxTotal}"/>
        <property name="maxIdle" value="${redis.maxIdle}"/>
        <property name="minIdle" value="${redis.minIdle}"/>
        <property name="maxWaitMillis" value="${redis.maxWaitMillis}"/>
        <property name="minEvictableIdleTimeMillis" value="${redis.minEvictableIdleTimeMillis}"/>
        <property name="softMinEvictableIdleTimeMillis" value="${redis.softMinEvictableIdleTimeMillis}"/>
        <property name="numTestsPerEvictionRun" value="${redis.numTestsPerEvictionRun}"/>
        <property name="timeBetweenEvictionRunsMillis" value="${redis.timeBetweenEvictionRunsMillis}"/>
    </bean>
    <bean id="jedisConnectionFactory"
          class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
        <property name="poolConfig" ref="poolConfig"/>
        <property name="password" value="${redis.password}"/>
        <property name="port" value="${redis.port}"/>
        <property name="hostName" value="${redis.host}"/>
        <property name="timeout" value="${redis.timeout}"/>
        <property name="database" value="${redis.db}"/>
    </bean>
    <bean id="defaultSerializer"
          class="org.springframework.data.redis.serializer.StringRedisSerializer"/>
    <bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">
        <property name="connectionFactory" ref="jedisConnectionFactory"/>
        <property name="defaultSerializer" ref="defaultSerializer"/>
    </bean>
</beans>

```

redis.properitied
```yaml
redis.maxTotal=50
redis.maxIdle=50
redis.minIdle=50
redis.maxWaitMillis=1000
redis.minEvictableIdleTimeMillis=-1
redis.softMinEvictableIdleTimeMillis=300000
redis.numTestsPerEvictionRun=20
redis.timeBetweenEvictionRunsMillis=60000
redis.timeout=2000
redis.host=sh-nh-b2-3-m6-redis-9-61
redis.port=6385
redis.password=jredis123456
redis.db=6
```

redisTemplate注入
```java
    @Autowired
	private RedisTemplate<String, String> redisTemplate;
```