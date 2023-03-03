# 昨日总结

day3-选择，循环，数组。

选择：

![image-20221118120444171](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118120444171.png)



循环：

goto标签的命名规则，和变量命名规则一致。

![image-20221118120424486](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118120424486.png)



数组：

![image-20221118120357099](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118120357099.png)





# strn系列

![image-20221118120735514](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118120735514.png)

基本和str系列作用相同，但只需要处理其中一部分原字符串时，用带n的，并且要穿进去的参数。



## strncpy()

strcpy()函数会把字符串尾标识符\0也拷贝过去。

但由于strncpy()中，限制了拷贝前多少多少位的情况，所以目的数组后不会有\0

![image-20221118133105891](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118133105891.png)

贸然输出，会有很多的烫烫烫。



## strncmp()

比较字符数组的前多少个字符

前俩空可以直接填字符串常量

![image-20221118134912515](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118134912515.png)

当然，如果在指定字节count之前就读到了\0，就终止了，认为字符串结束了。

所以比较int型数组之类的，不能用这个，得用memcmp()





## strncat()

目的地是要放一个空间，不能放字符串常量

后面的空可以填字符串常量

cat前

![image-20221118135657806](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118135657806.png)

体会一下此处memset的效果，用法。

cat后

![image-20221118135726382](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118135726382.png)

可以看出，cat后会自动给你添一个尾结束符\0



# mem系列

![image-20221118133534818](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118133534818.png)

## memset()

可以将一段内存全部设置成某一个东西。但我们通常将其设置成0完成初始化/重置

memset的第三个参数，是多少多少字节，不是某某类型。是把字符一个字节一个字节赋值的。

![image-20221118135219995](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118135219995.png)

memset()中目的为某段内存的开始地方就行了。

填-1，就是全存成ff(因为存的是补码)

c+4，就相当于c[4]，第五个位置开始

1，填充的字节，填01

sizeof(c) - 4，由于c是char型数组，所以可以采用sizeof来统计所有字节，再减去前面四个元素的4字节。



## memcmp()

![image-20221118140712134](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118140712134.png)

比较前若干字节，int啥的都行。

memcmp与strncpy的区别在于，strncpy用来比较字符数组前n字符。

memcmp用于比较任何类型的数组，内部实现是逐字节的比较。

函数的第三个参数是，比较的前多少个字节数，size_t，就是字节。

## memcpy()

![image-20221118141224859](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118141224859.png)

这个更常用，当然也是逐字节，前面是目的数组，后面是源数组b+2就是从，b[2]开始拷贝12个字节

memcpy(a+2,b+2,12)

从后面的b+2，也就是b[2]中开始拷贝，拷贝12字节（int型3个数字），到a+2，即a[2]





## memmove()

![image-20221118141452231](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118141452231.png)

![image-20221118181646574](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118181646574.png)



memcpy已经不是当年的memcpy了，重叠的也可以搞了

memcpy(a+2,a,sizeof(int)*2);也行了。memmove也可以。



当自己实现memmove时，主要要判断to和from谁在前谁在后

from在to后时，要从from的刚开始开始。

from在to前时，要从后面拷。



















