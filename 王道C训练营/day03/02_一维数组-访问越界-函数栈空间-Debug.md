# 数组

数组与基本数据类型不能类比，

数组是一种数据类型，一种构造数据类型。

为了方便存取，我们可以借助C语言提供的数组，通过一个符号来访问多个元素

![image-20221110201720375](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221110201720375.png)



# 一维数组基本定义

## 数组的定义

![image-20221110201841077](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221110201841077.png)

相同的数据类型

使用过程中需要保留原始数据

![image-20221110202607229](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221110202607229.png)

数组的大小不依赖于程序运行过程中的变量的值



## 常见的声明错误

![image-20221110202701482](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221110202701482.png)

![image-20221110203936762](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221110203936762.png)

这也是错误。只有初始化的时候才能用{}来进行初始化赋值，初始化之后的赋值是不支持{}来赋值的



注意！！

没初始化，只声明，会给内存写出cccc

只有初始化操作了，才会默认给你后面附加\0，不附加\0输出会造成恐怖的后果哦~

所以建议声明时，就给出初始化为0

``` c
char arr[100] = {0};
```



## 数组在内存中

呃呃，注意在监视中查看数组内存，也是得加上&的，&arr

断点此句是到这一句停下来，此句是未执行的。

![image-20221110203308735](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221110203308735.png)

一次F10，完成赋值

![image-20221110214741613](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221110214741613.png)

可见微软对数组还是很谨慎的，后面给了8字节的cc填充保证一点点越界不造成很大事故

***数组是存在栈空间里的，数组下标小的在上面，大的在下面，连续的正向逻辑***

而栈空间中，先定义的变量地址在下面，后定义的在上面，先进后出的数据结构.

但这都建立在Win32的环境下，如果是x64，直接先定义的在前面，后定义的在后面了。。。

## 数组越界

初始化，先定义的在下面

![image-20221110215202109](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221110215202109.png)

然会随着步入，arr[5]等越界数组行为，会顺着数组往下蔓延，最终arr[7]覆盖掉的是比数组先定义的i的值

![image-20221110215352814](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221110215352814.png)

Linux下没有这个保护措施，越界1字节也会game over



Debug是调试，Release是发布

F10当前函数一步步往下走，逐过程

F11到达某函数你要进去的时候，要F11，逐语句



## 初始化方法

![image-20221110203831322](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221110203831322.png)



# 一维数组的存储及函数的传递



一维数组在做参数传递时，会退化成指针，指向数组首地址的指针。

因为你在子函数中进行所谓的sizeof(arr) / sizeof(int)   是不可以的。sizeof(arr)会只有4（x64时会是8）



所以传参时只要：

``` c
print(int arr[],int length){}
```

里面不需要标注具体数字，因为没用。



所以一维数组的传递，长度是要靠自己传的。



# 栈空间与数组

![image-20221110221013606](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221110221013606.png)

单个函数的栈空间大小很小。超过了会发生著名的Stack overflow。

常引起这种的原因是数组过大与递归。



调用函数就是新增一个栈空间，压栈，调用完就会释放掉，弹栈。所以先进后出显得很重要















