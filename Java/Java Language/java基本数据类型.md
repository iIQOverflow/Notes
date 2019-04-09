# java基本数据类型
## Integer
| 类型  | 存储空间 | 二进制 |
| ----- | ------- | ----- |
| byte  | 1byte   | 8位    |
| int   | 4bytes   | 32位   |
| short | 2bytes   | 16位   |
| long  | 8bytes   | 64位   |
* long类型后缀为L，16进制前缀0x，8进制前缀0，二进制前缀0b
## 浮点数（Floating-Point）
|  类型  | 存储空间 | 二进制 |
| ------ | ------- | ----- |
| float  | 4byte   | 32位   |
| double | 8bytes   | 64位   |
* float后缀为F，3.14F，double后缀为D，3.14D
* 16进制表示浮点数使用p，比如$0.125=2^{-3}$写作`0x1.0p-3`，而不是`0x1.0e-3`，此时基数为2，而不是10.
* 浮点数以出或者错误
    * 正无穷
    * 负无穷
    * NaN(not a number)
* 判断是否为Double且NaN：
```
if (Double.isNaN(x))
```
* 浮点数不适合金融运算，因为在操作系统中是用二进制存储的，只能无限逼近某个小数，比如
```
System.out.println(2.0 - 1.1)
```
结果为
```
0.8999999999999999
```
因为二进制中不能represent 1/10，就像十进制中不能represent 1/3一样，可以用BigDecimal来处理这类问题。
## char
* 16位，存储Unicode码，用单引号赋值（`'A'`而不是`"A"`)
* 不建议在java中使用char类型
* 可以用char类型存储密码，尽量不要用String，因为String对象不可变，垃圾回收可能暴露，具体参考[基于密码的加密](https://docs.oracle.com/javase/6/docs/technotes/guides/security/crypto/CryptoSpec.html#PBEEx)