![image-20221118142602467](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118142602467.png)

不要盲目的觉得指针很厉害，什么时候都想用。。。



# 编址

![image-20221118143236050](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118143236050.png)

框出来的其实是一种编址，编址的意思，呃，内存里面，没有去存这些地址。

就像一个教室里，几排几座，都是人们想象的，没有用什么东西写下来。

cpu针对他要访问的内存，他会有一个编址，像图中，cpu32根地址总线，（四字节），只要往这32根地址总线上用高低电平之类的信号一通知（每一根一位），假如32根地址总线体现的信息转换成16进制是00F9F5E9

他就能够告诉数据总线，把这个位置的数据，从数据总线拿出来。

这个编址其实是虚拟地址，至于虚拟地址翻译成现实地址那就是操作系统的事情了，暂时不管。



而指针，就是将这些编址存下来的一个变量。

我们对一个变量取地址时，得到的都是这个变量的起始地址。



# 指针的使用及传递

见回顾指针专门md文档





# 堆空间/malloc/calloc

真实的物理地址一般是不会直接暴露给程序员的，会非常的危险，一般都是用虚拟地址

你malloc（1《30）一个G的空间，程序也是说给就给的，至于你要用到时，才会有虚拟地址到现实地址的映射

申请的虚拟空间是连续的，映射到物理地址时大概率是不连续的

malloc返回的是一个void*的类型，所以最好在刚申请后就强转成需要的类型

p = （char*）malloc（length）；

```c
int length;
scanf("%d",&length);
p = (char*)malloc(length);
strcpy(p,"hello");
puts(p);
free(p);
```

微软释放后会把值转换成ee fe



需要注意的是，内存的释放free掉，是不能改变指针指向堆地址的起始地址的

如果你p++；的操作，再free（p）就会报错

如果要偏移，则需要存最初的位置，free时，给free的指针必须是最初malloc返回的。



指针直接指向字符串，该字符串存在常量区，是不能写入的。



堆空间不会随着函数结束而释放，需要手动释放，这也是他的魅力所在。

## calloc

calloc简而言之就相当于malloc加上memset 0

calloc有俩个参数，方便申请成员数组

前一个参数是成员大小，后一个是多少个，size

pstu p = （pstu）calloc（sizeof（stu），10）；



# 野指针

什么是野指针

野指针就是指向被释放的地方的指针，

也就是说你free掉了，但没有将指向该处的指针置NULL

这种情况，就是野指针



## 经典例子

![image-20221118175103016](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118175103016.png)

关键就在于free(p1);但没有p1 = NULL;

malloc里面free掉的空间没有真正还给内存，下一个指针申请时会尽可能重复利用

free只是标记这段内存可以被用了，并没有对这个内存进行怎么怎么操作。





# const

深入理解一下const

保存的是一些全局常量，编译期间可以修改，运行期间不能。

![image-20221118175914801](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118175914801.png)

![image-20221118180233491](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118180233491.png)

以上为const和#define区别



区分const char * ptr和char const * ptr和char * const ptr三者的区别

主要看const和*谁在前面

const在*前面，就是修饰指向的值是const的

*在const前面，就是const的是指针ptr，指向不能变了



针对const修饰指针，存在以下俩种情况

- const char *ptr；

![image-20221118180313181](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118180313181.png)

![image-20221118180604299](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118180604299.png)

ptr指向的值不能通过ptr改变，但可以改变ptr的指向。

- char * const ptr；

![image-20221118180542122](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118180542122.png)

![image-20221118181150680](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118181150680.png)

也就是，我们不能修改ptr的指向了。但可以修改ptr指向的值。





















