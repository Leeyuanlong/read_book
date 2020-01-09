### 知识点1：类加载机制
#### 1.类加载的过程，加载、验证、准备、解析、初始化。每个部分详细描述。

- 类加载器：
    - 1）虚拟机团队把类加载阶段中的“通过一个类的全限定名来获取类的二进制字节流”放到Java虚拟机外部实现，以便让应用程序自己决定如何去获取所需的类。
    - 2）每个类加载器都拥有一个独立的类名称空间。
    - 3）三种类加载器
      BootstrapClassLoader：启动类加载器。JVM一部分，加载<JAVA_HOME>\lib目录中的，或者被-Xbootclasspath参数指定的路径中的类库。无法被java程序直接饮用。
      ExtensionClassLoader：扩展类加载器。加载<JAVA_HOME>\lib\ext目录中的，或者被java.ext.dirs系统变量所指定的路径中的所有类库。开发者可以直接使用扩展类加载器。
      ApplicationClassLoader：应用程序加载类或称为系统类加载器。这个类加载器是ClassLoader中的getSystemClassLoader()方法的返回值。加载用户类路径上所指定的类库。开发者可以直接使用这个类库。如果应用程序没有自定义过自己的类加载器，一般情况作为程序中的默认类加载器。

- 双亲委派机制
![双亲委派机制](https://github.com/Leeyuanlong/pict_bank/blob/master/java/deep_leraning_jvm/%E5%8F%8C%E4%BA%B2%E5%A7%94%E6%B4%BE%E6%9C%BA%E5%88%B6.png)


加载：类加载器根据一个类的全限定名来读取二级制字节流到JVM中，将这个字节流代表的静态存储结构转化为方法区的运行时数据结构，然后转换为一个与目标类对应的java.lang.Class对象。
    

链接：
    1）验证：格式检查、元数据验证、字节码验证、符号引用验证
    2）准备：对类变量分配空间，并设置初始值
    3）解析：符号引用转换为直接引用，得到类或者字段、方法在内存中的指针或者偏移量，以便直接调用该方法。

初始化：<clinit>方法被执行，初始化类变量及static代码块。该方法由JVM调用。JVM确保在多线程情况下同步。

#### 2.类卸载的过程及触发条件。
1）该类所有实例全部被回收
2）该类对应的Class对象，没有任何地方引用
3）加载该类的ClassLoader被回收

由JVM自带的类加载器加载的类，在JVM生命周期内始终不会被卸载。JVM本身会始终引用这些类加载器。由用户自定义的类加载器加载的类可以被卸载。

实例始终会引用类的Class对象，Class对象始终会引用ClassLoader对象。

FullGC的时候会卸载类。

#### 3.如何自定义一个类加载器？

```java
import java.io.FileInputStream;
import java.lang.reflect.Method;

public class Main {
    static class MyClassLoader extends ClassLoader {
        private String classPath;


        public MyClassLoader(String classPath) {
            this.classPath = classPath;
        }


        private byte[] loadByte(String name) throws Exception {
            name = name.replaceAll("\\.", "/");
            FileInputStream fis = new FileInputStream(classPath + "/" + name
                    + ".class");
            int len = fis.available();
            byte[] data = new byte[len];
            fis.read(data);
            fis.close();
            return data;
        }


        protected Class<?> findClass(String name) throws ClassNotFoundException {
            try {
                byte[] data = loadByte(name);
                return defineClass(name, data, 0, data.length);
            } catch (Exception e) {
                e.printStackTrace();
                throw new ClassNotFoundException();
            }
        }

    };


    public static void main(String args[]) throws Exception {
        MyClassLoader classLoader = new MyClassLoader("D:/test");
        Class clazz = classLoader.loadClass("com.huachao.cl.Test");
        Object obj = clazz.newInstance();
        Method helloMethod = clazz.getDeclaredMethod("hello", null);
        helloMethod.invoke(obj, null);
    }
}
```


#### 4.验证过程是防止什么问题？验证过程是怎样的？加载和验证的执行顺序？符号引用的含义？
检查字节流文件符合JVM规范。包含四种验证：1）格式验证；2）元数据验证；3）字节码验证；4）符号引用验证

加载和验证的顺序互相交叉进行

符号引用的含义：一组符号描述所引用的目标，eg:CONSTANT_Classref_info、CONSTANT_Fieldref_info、CONSTANT_Methodref_info

#### 5.加载阶段读入.class文件，为什么需要使用二进制的方式？
不知道啊

#### 6.准备过程的静态成员变量分配空间和设置初始值问题。
对类变量设置初始值，零值。
分配在方法区。
final常量为相应的值。

#### 7.解析过程符号引用替代为直接引用细节相关。

#### 8.初始化过程jvm的显式初始化相关
调用<clinit>方法，该方法是编译期收集类变量及static语句块生成的。

### 知识点2：类结构
1.Java类结构是怎么样的？
8位字节为一组的二进制流

文件由无符号数和表组成

### 知识点3：对象分配、回收
1.Java中垃圾回收机制GC原理等
1）判断对象是否死亡
2）垃圾收集算法
3）垃圾收集器
4）GC过程：1）MinorGC、FullGC

3.Java虚拟机的栈内存里面都放了什么？
栈是线程私有的空间，描述的是方法执行的方式。栈中存放的是栈帧。

4.CMS回收器、G1回收器介绍

5.paralle Scavenge高吞吐量

6.Java内存模型，五个部分，程序计数器、栈、本地栈、堆、方法区。每个部分的概念、特点、作用。



7.JVM内存分配策略，优先放于eden区、动态对象年龄判断、分配担保策略等。

8.四类引用及使用场景？

### 知识点4：执行引擎

