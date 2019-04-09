# java编程风格
## 变量
* 变量名尽量有意义。
* 声明变量后记得使用前进行初始化。
* **变量声明尽可能靠近它们第一次被用到的代码**。
## 常量
声明一个常量使用关键字`final`，**声明后只能设置一次值**，通常大写。

在一个类中声明的常量可以被不同的方法调用，示例如下：
```
public class Constants2
{
    public static final double CM_PER_INCH = 2.54;
    public static void main(String[] args)
    {
        double paperWidth = 8.5;
        double paperHeight = 11;
        System.out.println("Paper size in centimeters: "
        + paperWidth * CM_PER_INCH + " by " + paperHeight * CM_PER_INCH);
    }
}
```
这里声明为`public`，所以其他类也可以调用`CM_PER_INCH`，如`Constants2.CM_PER_INCH`。
## 自增/自减运算符
尽量不要在表达式内使用自增/自减运算符。
