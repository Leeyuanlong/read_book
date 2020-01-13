# 深入理解String类

### 总结
```text
1.String类是final类，类中的成员方法默认为final方法。
2.String类是通过char数组来保存字符串的。
3.String对象一旦被创建就是固定不变的了，对String对象的任何改变都不影响到原对象，相关的任何change操作都会生成新的对象。
无论是substring、concat还是replace操作都不是在原有的字符串上进行的，而是重新生成了一个新的字符串对象。这些操作后，最原始的字符串并没有被改变。
4.用new String() 创建的字符串不是常量，不能在编译期就确定，所以new String() 创建的字符串不放入常量池中，它们有自己的地址空间。
5.对于final修饰的变量，它在编译时被解析为常量值的一个本地拷贝存储到自己的常量池中或嵌入到它的字节码流中。
6.String、StringBuffer、StringBuilder的区别
（1）可变与不可变：String是不可变字符串对象，StringBuilder和StringBuffer是可变字符串对象（其内部的字符数组长度可变）。
（2）是否多线程安全：String中的对象是不可变的，也就可以理解为常量，显然线程安全。StringBuffer 与 StringBuilder 中的方法和功能完全是等价
的，只是StringBuffer 中的方法大都采用了synchronized 关键字进行修饰，因此是线程安全的，而 StringBuilder 没有这个修饰，可以被认为是非线程安全的。
（3）String、StringBuilder、StringBuffer三者的执行效率：
StringBuilder > StringBuffer > String 当然这个是相对的，不一定在所有情况下都是这样。比如String str = "hello"+ "world"的效率就比 
StringBuilder st  = new StringBuilder().append("hello").append("world")要高。因此，这三个类是各有利弊，应当根据不同的情况来进行选择使用：
当字符串相加操作或者改动较少的情况下，建议使用 String str="hello"这种形式；
当字符串相加操作较多的情况下，建议使用StringBuilder，如果采用了多线程，则使用StringBuffer。

7.String的intern()方法就是扩充常量池的一个方法；当一个String实例str调用intern()方法时，java查找常量池中是否有相同unicode的字符串常量，
如果有，则返回其引用，如果没有，则在常量池中增加一个unicode等于str的字符串并返回它的引用。
```


### `API`

#### org.apache.commons.lang3.StringUtils
- StringUtils.rightPad(string, totalCnt, charFilled)
    右侧填充
   
- StringUtils.leftPad(string, totalCnt, charFilled)
    左侧填充

- StringUtils.center(string, totalCnt, charFilled)
    两侧填充
    



