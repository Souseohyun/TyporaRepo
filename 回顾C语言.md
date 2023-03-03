## 前言

为了考研专业课铺底的同时，我也在俩年学习中各种编程语言选择方向中逐渐明白了自己所想的。

是的，我貌似更热爱底层的，原理的，高性能的编程，我并不看好接下来CN的Web开发就业环境。

于是我最终选择了C/C++，并暗暗给自己打气，一定要坚持学习他。





# 回顾一些语法

简易而又通用的各类基础语法便直接略过，差不多直接选择从结构开始回顾。

## 数据类型：

什么是数据类型？为什么需要数据类型?

数据类型是为了更好进行内存的管理,让编译器能确定分配多少内存。

数据类型非常重要，它可以告诉编译器分配多少内存可以放得下我们的数据。



顺带抠一下常识细节，常用数据类型及大小：

**数据类型**的大小一般会和机器位数（32/64）挂钩，

像long这类数据类型，跟编译器的数据模型也有关系。

一般情况下Windows64位操作系统采用LLP64模型，4字节长

而64位的Linux、Unix一般会使用LP64模型，需要特殊记忆一下：8字节长



| Datetype  | LP64 | ILP64 | LLP64 | ILP32 | LP32 |
| :-------- | :--- | :---- | :---- | :---- | :--- |
| char      | 8    | 8     | 8     | 8     | 8    |
| short     | 16   | 16    | 16    | 16    | 16   |
| int       | 32   | 64    | 32    | 32    | 16   |
| long      | 64   | 64    | 32    | 32    | 32   |
| long long | 64   |       |       |       |      |
| pointer   | 64   | 64    | 64    | 32    | 32   |

常用**Windows64**位数据汇总：

字符型：

char	1B;		



整形：				

short	2B;		

int	 	4B;		

long	  4B;		

long long  8B;	



浮点型：

float	  4B;

double  8B;



指针：

8字节

注意C语言中没有String这类型数据



## 控制符

一定要预想好什么类型数据 和 什么类型数据 在运算，会变成什么类型数据

就像pow（）函数，返回值是double类，即使计算pow（10，1）也会变成double数据，将直接影响后续计算

或者导致和控制字符不衔接，直接出现不报错的失误。

例如简单的：

```c
printf("%d", pow(10,1));
```

pow(10,1)返回一个浮点型数据，浮点型数据存储方式（类ieee754等）和整形（补码）完全不一样，

将计算出来的二进制数据用整形%d接收，就会酿成惨剧

答案显示为：0；

![image-20221101191605276](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221101191605276.png)



即：使用%d整形接收浮点型数据，不会自动截尾也不会给你四舍五入，会出现无法挽回的错误！！还不报错！

在各种计算中，要及时强转最好保证类型的统一，或在自己把握范围之内。

```c
//判断是否为对称int型关键代码段，及时强转
if((number /(int)pow(10,i-1)) % 10 == number / (int)pow(10,length-1) % 10){
(number /(int)pow(10,i-1)) % 10     √         number / (int)pow(10,length-1) % 10
        //123321  %    100  = 21   x                 123321  /  100000  =  1
        // 从后取  123321 /1  % 10   1     从前取      123321  /  10000   =12  %  10=2
        // 123321 /10 % 10          2                123321  /  1000    =123 % 10 =3
        // 123321 /100 %10          3
    printf("yes  %d\n",i);
}
else{
    printf("no  %d\n",i);return -1;}
```



## struct/typedef

struct便是用来定义一种数据类型，自定义的一种数据结构，简化结构体的关键字

typedef就是用来给**类型**起别名的，语法：typedef 原名 别名   typedef int MYint；

```C
typedef struct Person{
    char name[20];      //易混点：C语言中声明数组char name[10]而非char[10] name；
    int age;
}myPerson;

typedef struct Person youPerson;        //此处struct不能忘，或者说，没有typedef之前，struct和原										  //名几乎绑定

int main() {
    struct Person p1 = {"张三",20};		//三种声明方式，不要老想着java的new
    myPerson p2 = {"李四",21};
    youPerson p3 = {"王五",22};
    return 0;
}
```

说到结构体，免不了结构体引用.和->区分

一言蔽之，.用于实例引用，p1.name

​					->用于指针引用

```C
myPerson p1 = {"张三",20};
printf("%s\n",p1.name);
myPerson *p = &p1;
printf("%s",p->name);
```





### 区分数据类型

```C
void test01(){
    char * p1,p2;
    typedef char* PCHAR;
    PCHAR P3,P4;		//当然，也可以写成：char *p3，*p4；
    printf("sizeof p1:%d; p2:%d",sizeof(p1),sizeof(p2));	//sizeof p1:8; p2:1
    printf("sizeof p3:%d; p4:%d",sizeof(P3),sizeof(P4));	//sizeof p3:8; p4:8
    
}
```



p1是指针类型，p2是char类型；新手容易犯错。

所以说，人们习惯写成：char *p1，p2；是有一定理由的啊

重命名后，再声明，自然俩者都采用char*型数据类型。



## stdin缓冲区

C语言通过标准函数库实现的，C语言通过scanf读取键盘输入，键盘输入又被称为标准输入，当scanf函数读取标准输入时，如果还没有输入任何内容，那么将会阻塞，

``` c
int main() {

	int i = 0;
	char c;
	scanf("%d", &i);
	printf("%d", i);
    //fflush(stdin);
	scanf("%c", &c);
	printf("%c", c);

	system("pause");
	return 0;
}

```

![image-20221104132130898](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221104132130898.png)

第二个scanf函数将不会阻塞直接越过。scanf函数以%c控制符时，将接收例如空格，回车等输入流

scanf函数在读取整形，浮点型，字符串时，会“忽略”回车符，空格符。

***关键知识***

![image-20221104132633142](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221104132633142.png)

故混合输入多种类型时，%c与其他类型前要打入一个空格来防止键入空格被当作字符接收

![image-20221104133110362](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221104133110362.png)



![image-20221104133124523](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221104133124523.png)





## void 

``` c
//1. void修饰函数参数和函数返回
void test01(void){
	printf("hello world");		//若参数不写void，则外界传参进来时不会报错，只是传不进来而已。
    							//void和return 10不冲突，只会警告，只是不返回而已。
}

//2. 不能定义void类型变量
void test02(){
	void val; //报错
}

//3. void* 可以指向任何类型的数据，被称为万能指针
void test03(){
	int a = 10;
	void* p = NULL;
	p = &a;				//可以指向任何类型元素，当然也包括自定义的结构体
    printf("a:%d\n",*(int*)p);		//不可以正常输出，*p，要强转
    
    void *px = &p1;
    printf("%s\n",((myPerson*)px)->name)
        
						//但如果指向结构体，则不能正常使用->取出成员变量，要强转
	
	char c = 'a';
	p = &c;				
	printf("c:%c\n",*(char*)p);
}

//4. void* 常用于数据类型的封装
void test04(){
	//void * memcpy(void * _Dst, const void * _Src, size_t _Size);
}
```





## sizeof

```C
//1、sizeof的本质，是不是一个函数？？不是函数，只是一个操作符，类似+-*/
void test01(){
    //对于数据类型，sizeof必须用（）使用，对于变量，可以不加（）
    printf("size of int = %d\n",sizeof(int));
    double d = 13.14;
    printf("size of d = %d",sizeof d);
}
```



```C
//2、sizeof的返回值类型是什么？  unsigned int 无符号整形  被视为xx byte
void test02(){
    unsigned int a =10;
    if(a - 20 > 0){
        printf("大于0\n");      //将输出该句，unsigned int和int做运算，编译器自动都转成unsigned int
    } else{
        printf("小于0");
    }
    if(sizeof(int) - 5 >0){
        printf("大于0  %d\n",sizeof(int) - 5);    //输出为-1，%d是有符号整形输出格式，%u才是无符号整形输出格式
    }else{
        printf("小于0");
    }

}
```



``` c
//3、sizeof可以统计数组长度
//当数组名作为函数参数时候，会退化为指针，指向数组中第一个元素
void calculateArray( int arr[] )
{
	printf("arr的数组长度： %d\n", sizeof(arr));		//返回8，原因如上，指针的大小为8
}

void test03()
{
	int arr[] = { 1, 2, 3, 4, 5, 6, 7, 8 };

	//printf("arr的数组长度： %d\n", sizeof(arr));  返回正确值：32
    //可以用sizeof（arr）/sizeof（int）统计个数，不用循环。但如果是传参，会退化成指针

	calculateArray(arr);
}
```



## 变量的修改方式



```C
//对于常规数据类型
void test01()
{
   int a = 10;
   //直接修改
   a = 20;
   printf("a = %d\n", a);
   //间接修改
   int * p = &a;
   *p = 100;

   printf("a = %d\n", a);

}
```



```c
//对于自定义数据类型
struct Person
{
   char a; // 0 ~ 3		为什么此处是0~3呢，见下方说明：内存对齐
   int b;  // 4 ~ 7
   char c; // 8 ~ 11
   int d;  // 12 ~ 15
};
void test02()
{
   struct Person p1 = { 'a', 10, 'b', 20 };

   //直接修改  d 属性
   p1.d = 1000;

   //间接修改  d 属性
   struct Person * p = &p1;
   p->d = 2000;
    
   //倘若想通过偏移量修改：
   //printf("%d\n", p);		输出可知p和p+1相差16
   //printf("%d\n", p+1);	p是Person型指针，+1则会跳一个完整的结构体长度。不合题意

   char * pPerson = p;		//再创建一个char型指针（char仅1B），

   printf("d = %d\n", *(int*)(pPerson + 12));//因为要一次性取出的d是int型（4B），所以需要强转指针
   printf("d = %d\n",  *(int*)((int*)pPerson +3) );//做算术题，int4B，3x4=12，故只需+3；
}
```



## 内存对齐

#### 步骤：

1. 结构体各成员对齐.
2. 结构体总体对齐



#### 规则：

结构体(struct)的数据成员,第一个数据成员存放的地址为结构体变量偏移量为0的地址处.



其他结构体成员自身对齐时,存放的地址为min{自身对齐值, 指定对齐值} 的最小整数倍的地址处.
注:自身对齐值:结构体变量里每个成员的自身大小
注:指定对齐值:有宏 #pragma pack(N) 指定的值,这里面的 N一定是2的幂次方.如1,2,4,8,16等.如果没有通过宏那么在32位Linux主机上默认指定对齐值为4,64位的默认对齐值为8,AMR CPU默认指定对齐值为8;
注:有效对齐值:结构体成员自身对齐时有效对齐值为自身对齐值与指定对齐值中 较小的一个.

Cpp可以使用alignof(结构体)得到对齐系数



总体对齐时,字节大小是min{所有成员中自身对齐值最大的, 指定对齐值} 的整数倍.

#pragma pack(N) 每个特定平台上的编译器都有自己的默认“对齐系数”(也叫对齐模数)。程序员可以通过预编译命令#pragma pack(n)，n=1,2,4,8,16来改变这一系数，其中的n就是你要指定的“对齐系数”。



#### 示例：

示例1：

```C
struct Person
{
    char a; // 0 ~ 3
    int b;  // 4 ~ 7
    char c; // 8 ~ 11
    int d;  // 12 ~ 15
};
```

**结构体成员对齐：**

char占1字节，第一个结构体成员，放在偏移量为0的地址处

int	占4字节，第二个结构体成员，b的有效对齐值=min{4，8}=4

依次查看4的整数倍地址是否可以存放俩个字节，发现4可以。

此时内存占用情况为：（x为填充数据）

数据：a x x x b b b b

内存：0 1 2 3 4 5 6 7

char占1字节，第三个结构体成员，c的有效对齐值=min{1，8}=1

依次查看1的整数倍地址是否可以存放一个字节，发现8恰好可以

此时内存占用情况为：（x为填充数据）

数据：a x x x b b b b c

内存：0 1 2 3 4 5 6 7 8

int	占4字节，第四个结构体成员，d的有效对齐值=min{4，8}=4

依次查看4的整数倍地址是否可以存放四个字节，发现12可以。

此时内存占用情况为：（x为填充数据）

数据：a x x x b b b b c x  x     x    d    d    d    d

内存：0 1 2 3 4 5 6 7 8 9  10  11  12  13  14  15  16  17



**结构体整体对齐**

整体对齐时，字节大小是：

min{所有成员自身对其的最大值，指定对齐值}的整数倍=min{4，8}的整数倍=4的整数倍

此时16恰是4的整数倍，故整体对齐不需要调整。

故sizeof（struct Person）= 16



示例2：

``` c
typedef struct _st_struct2
 {
 	char 	a;
	int		c;
	short   b;
 }st_struct2;
```

**成员对齐：**

char   a	0~

int      c	4~7

short b	8~9

共10字节

**整体对齐：**

n = min{4，8} = 4，最终结构体大小需是4的整数倍

故再填充俩字节，达到12字节，sizeof（st_struct2） = 12

若单纯sizeof（p.a)则会=1，填充字段不算在内。





## const/static

















## memset（）

函数所需头文件：

``` c
#include <string.h>
```

函数原型：

``` c
void *memset(void *str, int c, size_t n)
```

注解：

str		指向目标的指针

c		   要设置的字符

n		   字符数

原意：将str的前n个字符设置成**字符**c



**注意事项**

memset()是以字节为单位进行作用的。

所以用memset()对   ***非字符型数组，非字符型指针***   赋初值是非常不好的。

举例：

```c
void test01(){
    int arr[5] = {1,2,3,4,5};
    memset(arr,1,5*sizeof(int));	//指向目标的指针、设置成的字符、前多少个字符发送改变
    for (int i = 0; i < 5; ++i) {
        printf("%d   ",arr[i]);
    }
}

int main(){
    test01();
    return 0;
}
```

![image-20221101133218773](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221101133218773.png)

为什么会是：16843009呢？

要牢记memset()是对字符进行操作，自然是以字节作为单位操作。

5*sizeof int ;一共20字节操作长度

int型数组中，每个成员都是int型数据（4B）

拿第一个数组成员举例：1

0000 0000 0000 0001			（memset前）

​      		-->

0001 0001 0001 0001			  （memset后）		=16843009

故，不要被函数源码中void *memset(void *str, int c, size_t n)

的int c 所迷惑，要把传进去的参数当成字符，这个函数也是对字符进行操作，后面的size也基于字符单位



举例用法：

```c
void allocateSpace( char * pp ){
   char * temp =  malloc(100);
   if (temp == NULL){
      return;
   }
   memset(temp, 0, 100);	//赋初值是最常用的
   strcpy(temp, "hello world");
   pp = temp;

}
```











# 内存分区模型



### 内存分区

#### 运行之前：

我们要想执行我们编写的c程序，那么第一步需要对这个程序进行编译。

处理步骤：

1）预处理：宏定义展开、头文件展开、条件编译，这里并不会检查语法

2）编译：检查语法，将预处理后文件编译生成汇编文件

3）汇编：将汇编文件生成目标文件(二进制文件) .o后缀

4）链接：将目标文件链接为可执行程序





当我们编译完成生成可执行文件之后，我们通过在linux下size命令可以查看一个可执行二进制文件基本情况：

![img](file:///C:\Users\23614\AppData\Local\Temp\ksohtml19216\wps1.jpg) 

通过上图可以得知，在没有运行程序前，也就是说程序没有加载到内存前，

可执行程序内部已经分好3段信息，

分别为代码区（text）、

静态数据区（data） 、（全局已初始化数据区）

未初始化数据区（bss）（全局未初始化数据区）

3 个部分（有些人直接把data和bss合起来叫做静态区或全局区）。

![](C:\Users\23614\Desktop\C语言\image\内存分区01.png)



![](C:\Users\23614\Desktop\C语言\image\程序指令03.png)



**为什么要把程序的指令和数据分开呢？**



![](C:\Users\23614\Desktop\C语言\image\内存分区02.png)





#### 运行之后



程序在加载到内存前，代码区和全局区(data和bss)的大小就是固定的，程序运行期间不能改变。

然后，运行可执行程序，操作系统把物理硬盘程序load(加载)到内存，

除了根据可执行程序的信息分出代码区（text）、数据区（data）和未初始化数据区（bss）之外，

**还额外增加了栈区、堆区。**

（栈区，堆区，都是在程序运行后才创建出来，是人为划分出来的一个区）

**栈区概述：**

​				先进后出的数据结构

​				由编译器管理数据开辟和释放

​				变量的生命周期在该函数结束后自动释放

​				局部变量，指针，数组，数组成员

**堆区概述：**

​				容量远远大于栈区

​				没有先进后出这样的结构

​				堆区是由程序员管理开辟和管理释放

​								malloc			free

​				手动开辟，最好手动释放，若无手动释放，程序全部结束后，系统统一回收

​				malloc开辟出来的空间、

![](C:\Users\23614\Desktop\C语言\image\内存分区04.png)







### 栈区注意点

1.1 不要返回局部变量的地址，

因为局部变量在函数执行之后就释放了，

我们没有权限取操作释放后的内存

```C
//栈 注意事项 ，不要返回局部变量的地址，函数运行完，内存会释放。
int * func()
{
   int a = 10;
   return &a;
}

void test01()
{
   int * p = func();

   //结果已经不重要了，因为a的内存已经被释放了，我们没有权限去操作这块内存
   printf("a = %d\n", *p);
   printf("a = %d\n", *p);
}
```



```C
//同理，str也存放在栈区，
char * getString()
{
   char str[] = "hello world";		
   printf("%d\n",sizeof(str));		//13, 证明整个hello world都被塞进了栈区，命名为str，
                                    //    而非首地址引用
   return str;
}

void test02()
{
   char * p = NULL;
   p = getString();
   printf("%s\n", p);
}
```

![IMG_3755](C:\Users\23614\Desktop\C语言\image\IMG_3755.JPG)





### 堆区注意点

堆区正常使用方法：

```c
int * getSpace()
{
    //通过malloc开辟空间存放
   int * p = malloc(sizeof(int)* 5);

   if (p == NULL)
   {
      return NULL;
   }

   for (int i = 0; i < 5;i++)
   {
      p[i] = i + 100;				//神奇的使用技巧，把指针当数组使
      //本句平替：*(p + i) = i + 100;
   }
   return p;

}

void test01()
{
   int * p = getSpace();

   for (int i = 0; i < 5;i++)
   {
      printf("%d\n", p[i]);
      //平替：printf("%d\n",*(p + i));
   }

   //手动在堆区创建的数据，记得手动释放
   free(p);
   p = NULL;		//防止野指针

}
```



注意点：

```c
//注意事项
//主调函数中作为实参的指针，被调函数若用同级指针接收，是无法修改实参指针的指向的。
void allocateSpace( char * pp )
{
	char * temp =  malloc(100);
	if (temp == NULL)
	{
		return;
	}
	memset(temp, 0, 100);
	strcpy(temp, "hello world");
	pp = temp;

}

void test02()
{
	char * p = NULL;
	allocateSpace(p);

	printf("%s\n", p);
}

```



内存图解：

![image-20221101134849759](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221101134849759.png)

即：

主调函数中，指针作为实参传入，被调函数用形参接收，

这个形参是再在栈区新建一个内存空间pp，并让pp接收实参指针p的值（本例为null）

他们俩是同级关系，同等级别。

pp不是和主调函数中p指针用同一个内存空间，

这也导致了，在被调函数中修改形参pp的指向，和p没任何关系，程序跑完了，p仍然是null。

本质上和问题：值传递/地址传递	是一致的。



总结：

主调函数中作为实参的指针，被调函数若用同级指针接收，是无法修改实参指针的指向的。



**解决方法**：

```c
void allocateSpace( char ** pp )     //char **pp = &p;
{
    char * temp =  malloc(100);
    if (temp == NULL)
    {
        return;
    }
    memset(temp, 0, 100);
    strcpy(temp, "hello world");
    *pp = temp;                     //解绑1级指针，实际的到p指针，相当于实参p = temp;

}
void test02()
{
    char * p = NULL;
    allocateSpace(&p);			//传参应传该指针的地址（其实无论类型，想要改变该参数，都要传地址）

    printf("%s\n", p);
}
```



内存图解：

![image-20221101140540048](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221101140540048.png)

最终是的p内存处存0x001指向堆区。





### 数据区注意点

数据区放入的是静态变量、全局变量、常量



与局部变量区别，局部变量定义方法：

​		　普通的局部变量定义的时候直接定义或者在前面加上auto

``` c
#include <stdio.h>

int a = 10;		//这是全局变量，在C语言中，全局变量一般隐藏了前缀extern；
static int b = 100;		//这是静态全局变量，只能在本文件全局中使用。

void test(){
    int c = 10;		//这是局部变量，全存在栈里
    static int d = 15;		//这是静态局部变量，第一次调用时才初始化
}

```







***静态变量	与	局部静态变量***	辨析：

``` c
//1、静态变量
static int a = 10; //特点：只初始化一次，在编译阶段就分配内存，属于内部链接属性，只能在当前文件中使用
					
void test01()
{
   static int b = 20; //局部静态变量,能复用，第一次调用时初始化，作用域只能在当前test01中
						//而静态局部变量只对定义自己的函数体始终可见。
   //a 和 b的生命周期是一样的
}

```

静态局部变量和普通局部变量不同。

静态局部变量也是定义在函数内部的，

**静态局部变量所在的函数在多调用多次时，只有第一次才经历变量定义和初始化，**！！！

（注意，他并不是编译阶段便分配内存，初始化在第一次调用时初始化。）

以后多次在调用时不再定义和初始化，而是**维持之前上一次调用时执行后这个变量的值**。本次接着来使用。

总结：

静态局部变量	和	全局变量区别：

静态局部变量的这种特性，和全局变量非常类似。

它们的相同点是都创造和初始化一次，以后调用时值保持上次的不变。不同点在于作用域不同







**static 和 extern 的区别**

```c
//1、静态变量
static int a = 10; //特点：只初始化一次，在编译阶段就分配内存，属于内部链接属性，只能在当前文件中使用

void test01()
{
   static int b = 20; //局部静态变量,能复用，作用域只能在当前test01中

   //a 和 b的生命周期是一样的
}

```







