# 异常机制

## 概述
   异常类型
   自定义异常

## 本质
   Java采用面向对象的方式来处理异常的。本质：当程序出现错误，程序安全退出的机制。
   
## 异常处理过程
   1.抛出异常
    在执行一个方法时，如果发生异常，则这个方法生成代表该异常的一个对象，停止当前执行路径，并把异常对象提交给JRE。
   
   2.捕获异常 
    JRE得到该异常后，寻找相应的代码来处理该异常。JRE在方法的调用栈中查找，从生成异常的方法开始回溯，直到找到相应的一场处理代码为止。
   
## 异常分类
    所有异常对象都是派生于java.lang.Throwable类
    
### Error
    是程序无法处理的错误，Error表明系统JVM已经处于不可恢复的崩溃状态中。应用程序本身无法克服和恢复的一种严重问题。
    
### Exception

#### 编译器已检查的异常：CheckedException
   例如，网络断线，硬盘空间不够，发生这样的异常后，程序不应该死掉。编译器强制普通异常必须 try..catch 处理或用 throws 声明继续抛给上层调用方
   法处理，所以普通异常也称为checked异常。
    IOException
    SQLException
    
#### 运行时异常：RuntimeException
   任何运行时异常，都是程序员的疏忽大意。系统异常可以处理也可以不处理，所以，编译器不强制用 try..catch 处理或用throws声明，所以系统异常也称
   为unchecked异常。
   
   - 1）java.lang.NullPointerException 
   - 2）java.lang.ClassNotFoundException 
   - 3）java.lang.NumberFormatException 
   - 4）java.lang.IndexOutOfBoundsException 
   - 5）java.lang.IllegalArgumentException 
   - 6）java.lang.ClassCastException


## 使用异常机制注意事项
   - 1.要避免使用异常处理代替错误处理，会降低程序的清晰性，影响性能
   - 2.处理异常不可以代替简单测试---只有在异常情况下使用异常机制
   - 3.不要进行小粒度的异常处理---应该将整个任务包装在一个try块中
   - 4.异常往往在最高层处理


## 自定义异常
   - 1.遇到JDK无法描述清楚想表达的异常
   - 2.自定义异常类只需要从Exception类或者它的子类派生一个子类即可
   - 3.如果继承Exception类，则为受检查异常，必须对其进行处理。如果不想处理，可以让自定义异常继承运行时异常RuntimeException
   - 4.习惯上自定义异常应该包含2个构造器：1）默认构造器；2）带有详细信息的构造器

```java
class IllegalAgeException extends RuntimeException {
   	public IllegalAgeException() {
   `
   	}
   
   	public IllegalAgeException(String message) {
   		super(message);
   	}
}
```

## 补充

java中的异常的超类是`java.lang.Throwable`(后文省略为Throwable),它有两个比较重要的子类,`java.lang.Exception`(后文省略为Exception)和
`java.lang.Error`(后文省略为Error)，其中Error由JVM虚拟机进行管理,如我们所熟知的`OutOfMemoryError`异常等，所以我们本文不关注Error异常，
那么我们细说一下Exception异常。

Exception异常有个比较重要的子类，叫做`RuntimeException`。我们将RuntimeException或其他继承自`RuntimeException`的子类称为非受检异
常(unchecked Exception)，其他继承自Exception异常的子类称为受检异常(checked Exception)。

如果在一个应用中，需要开发一个方法(如某个功能的service方法)，这个方法如果中间可能出现异常，那么你需要考虑这个异常出现之后是否调用者可以处理，
并且你是否希望调用者进行处理，如果调用者可以处理，并且你也希望调用者进行处理，那么就要抛出受检异常，提醒调用者在使用你的方法时，考虑到如果抛出
异常时如果进行处理。

相似的，如果在写某个方法时，你认为这是个偶然异常，理论上说，你觉得运行时可能会碰到什么问题，而这些问题也许不是必然发生的，也不需要调用者显示的
通过异常来判断业务流程操作的，那么这时就可以使用一个RuntimeException这样的非受检异常.