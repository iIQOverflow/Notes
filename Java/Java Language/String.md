# String
## 不变性（Immutable）
```
String greeting = "Hello";
greeting = greeting.substring(0, 3) + "p!";
```
这里并非改变了greeting指向的String的值，而是通过String.substring()重新生成了一个新的String对象并让greeting指向这个新的对象。
## 不变性原因
应用程序不断扩展，String对象占用大量的存储空间，导致冗余，为了提高java的效率，JVM预留了一个称为“字符串常量池（String constant pool）”的特殊内存区域。

当编译器使用String对象时，它会首先在池中寻找String对象，如果匹配，直接调用，不创建已有的的String对象。已有的String对象可能有多个引用，这也是其Immutable原因：如果多个引用指向同一个String对象，一个引用更改了该String对象的值，那么其他引用就会受到影响，因为它们指向的都是同一个String对象。

如果有人override了String对象会怎样呢？这是不可能发生的，因为String声明为final，所以没人能override它的方法。
### 总结
* 提高效率，节省内存空间
* 线程安全
## String源码
String实际上就是对字符数组的封装。
```
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {

    private final byte[] value;
    private final byte coder;
    private int hash; // Default to 0

    public String() {
        this.value = "".value;
        this.coder = "".coder;
    }
}
```
## 使用注意
* String创建机制：，首先检查字符串池中是否有字面值相等的字符串，如果有，则不再创建，直接返回字符串池中该对象的引用，若没有创建之，然后放到字符串池中，并返回新建对象的引用。但是`new String("hello")`，直接声明一个String对象是不检查字符串池的，也不会把对象放到字符串池中。
* 开发中使用**直接量赋值**方式[**推荐**]，**除非确有必要才新建一个String的对象**。
* 使用"=="判断的是两个对象的**引用地址**是否相同，也就是判断是否为同一个对象。
* 放到字符串池中，不需要考虑垃圾回收问题。虽然Java的每个对象都保存在堆内存中，但是字符串池非常特殊，它的编译期已经决定了其存在JVM【虚拟机】的常量池，垃圾回收器是不会对它进行回收的。
## Reference
[第01篇 为什么推荐使用String直接赋值](https://www.cnblogs.com/pangxiansheng/p/5245086.html)

[在java中String类为什么要设计成final？](https://www.zhihu.com/question/31345592)

[Java中的String为什么是不可变的？ -- String源码分析](https://blog.csdn.net/zhangjg_blog/article/details/18319521)