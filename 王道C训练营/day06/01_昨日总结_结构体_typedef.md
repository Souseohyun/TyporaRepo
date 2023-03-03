# 昨日回顾

二级指针

![image-20221121092024586](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121092024586.png)

函数

![image-20221121092825501](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121092825501.png)

主要陌生的就是嵌套函数，setjmp，longjmp

以及各种变量的作用域需要深深的记住





# 结构体

![image-20221121093109540](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121093109540.png)



## 结构体定义引用初始化

![image-20221121093249497](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121093249497.png)

定义，声明一个类型，它不占空间的

可以放到头文件里

注意！！

结构体定义的最后要加上分号；

![image-20221121094143376](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121094143376.png)





## ￥￥内存对齐

```c
struct type {
	int height[3];
	double score;
	short age[3];
};//32
//计算结构体内存，内存合并
//gcc默认对齐系数是4，最大值不足4，按4算
//内存会是成员最大值的整数倍
//相邻的多个小可以合并,合并后不能超过最大的
//数组一定是顺序存储的，不留空隙的，只能后面补cc
```

多站在计算机的角度上思考，怎么一次，或少次的把数据取出

有数组时会稍微麻烦一点，最后记得验算，4的整数倍



定义结构体时，把小字节的变量定义在一起，放在一起或最后。 让他们内存合并，可以减少内存开销



## 结构体使用

清空结构体，如果一个个赋值0，实在是太长太麻烦了

可以想起我们之前的memset

![image-20221121110105618](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121110105618.png)

注意了，stu就是一个整合的变量，不是指针，memset要的是首地址，一定要取地址



用scanf给结构体赋值的时候，很容易因为%c吞空格的情况下，很容易匹配失败，及时加空格



## 结构体数组

![image-20221121111345138](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121111345138.png)

输入时要注意是否需要加取地址符，并且注意%c接收空格的事情

输出时可以控制格式

也可以这样连续的初始化

![image-20221121112238024](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121112238024.png)

输出时可以用.来取出结构体成员





## 结构体指针

结构体的取成员符.和->优先级都非常高，比指针符号高，所以要用指针的时候，记得把指针和*括起来

但这样写实在是太麻烦了，有指针的情况下，我们是用指针->来直接取出成员

![image-20221121114251648](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121114251648.png)



当结构体指针指向结构体数组时，直接指是可以的，反正数组名也是首地址

p=sarr

![image-20221121114641111](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121114641111.png)



当成员变量自增时：

num = p->num++时，前文说到过->的优先级几乎是最高的，比++的结合要高一级，故这个++是整体p->num这个成员变量++；

![image-20221121114922748](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121114922748.png)

![image-20221121115246807](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121115246807.png)



当指针自增时：
num = p++->num时，先num = p->num没问题，后续p++，是偏移一个结构体指针的字符数，直接跳到下一个结构体数组的num成员地址。

![image-20221121115757997](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121115757997.png)







# typedef

老是写struct student太麻烦，可以用typedef来起别名

typedef 原来名字 新名字；

typedef和#define的区别在于，#define在预编译阶段就完成了，typedef在编译阶段才会运行

![image-20221121120947019](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121120947019.png)





```c
typedef struct student {
	int num;
	char name[20];
	char sex;
}stu,* pstu;
//后续使用中pstu 等价于 struct student * 结构体指针类型

```

*pstu有点难以理解，多看看。

相当于定义了一个结构体指针

使用：

![image-20221121120656928](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121120656928.png)

是一样的，pstu就包含了指针了。

















