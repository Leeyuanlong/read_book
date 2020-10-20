# Tips

**资料**

- Spring AMQP documents:  https://docs.spring.io/spring-amqp/docs/2.1.0.RELEASE/reference/pdf/spring-amqp-reference.pdf

- 生产端应当只关心Exchange，少关心 Queue和binding。



**常用命令：**

- 启停命令

     启停 rabbitmq-server start &

​          停止 rabbitmqctl app_stop



**Channel与Connection的区别**

<font color=red>**Connection：高速公路   Channel：运输消息的卡车**</font>

Channel是我们与RabbitMQ打交道的最重要的一个接口，我们大部分的业务操作是在Channel这个接口中完成的，包括定义Queue、定义Exchange、绑定Queue与Exchange、发布消息等。如果每一次访问RabbitMQ都建立一个Connection，在消息量大的时候建立TCP Connection的开销将是巨大的，效率也较低。Channel是在connection内部建立的逻辑连接，如果应用程序支持多线程，通常每个thread创建单独的channel进行通讯，AMQP method包含了channel id帮助客户端和message broker识别channel，所以channel之间是完全隔离的。Channel作为轻量级的Connection极大减少了操作系统建立TCP connection的开销。

**ConnectionFactory**是RabbitMQ服务掌握连接Connection生杀大权的重要组件。有了它，就可以创建Connection(org.springframework.amqp.rabbit.connection.Connection)

CachingConnectionFactory是仅有的默认会创建一个能够被应用共享的连接代理，下图是CachingConnectionFactory的继承关系

Channel这个概念应该熟悉，可以认为是一个连接通道，CachingConnectionFactory中默认最多可以缓存25个Channel，也可以通过方法setChannelCacheSize()设置

注：channel 与 connection区别

A Connection represents a real TCP connection to the message broker, whereas aChannelis a virtual connection (AMPQ connection) inside it. This way you can use as many (virtual) connections as you want inside your application without overloading the broker with TCP connections.

You can use one Channel for everything. However, if you have multiple threads, it's suggested to use a different Channel for each thread.

There is no direct relation between Channel and Queue. A Channel is used to send AMQP commands to the broker. This can be the creation of a queue or similar, but these concepts are not tied together.

Consumer runs in its own thread allocated from the consumer thread pool. If multiple Consumers are subscribed to the same Queue, the broker uses round-robin to distribute the messages between them equally.

It is also possible to attach the same Consumer to multiple Queues. You can understand Consumers as callbacks. These are called everytime a message arrives on a Queue the Consumer is bound to. For the case of the Java Client, each Consumers has a methodhandleDelivery(...), which represents the callback method. What you typically do is, subclassDefaultConsumerand overridehandleDelivery(...). Note: If you attach the same Consumer instance to multiple queues, this method will be called by different threads. So take care of synchronization if necessary.



