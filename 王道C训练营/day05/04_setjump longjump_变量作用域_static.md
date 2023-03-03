# 协程调用



![image-20221120150929412](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120150929412.png)

连续调用多个函数，或者递归之类的多次，如果要返回，也是逐步，递归型一个个返回

但可以用setjmp和longjmp的接口一步到位，返回main函数中

用goto是不行的，goto不能跨函数跳转



## setjmp

![image-20221120151855247](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120151855247.png)

setjump/longjmp通过记录寄存器状态来“回到过去”

需要头文件#inlcude<setjmp.h>



将系统栈保存在envbuf中，以供后续调用longjmp()。

第一次使用setjmp(envbuf)时，会返回0.

```c
#include<setjmp.h>


jmp_buf envbuf;//env环境  buf当时现场,声明该变量记录标签

void b() {
	printf("i am b func\n");
	longjmp(envbuf, 5);//longjmp是要回到过去的，是没有返回值的，
								 //携带的信息就是5，作为回到过去后setjmp的返回值
}
void a() {
	printf("before b(),i am a func\n");
	b();
	printf("after b(),i am a func\n");
}
int main() {
	int i;
	i = setjmp(envbuf);//记录此时寄存器状态，存于envbuf中
	if (0 == i) {
		a();
	}
	printf("%d\n", i);
	system("pause");
	return 0;
}
```

setjmp保存现场，longjmp回到过去

当然，envbuf不一定非得是全局变量，也可以通过传参的形式来传给a，a传给b，b用于longjmp







# 递归调用

![image-20221120164203344](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120164203344.png)

递归做面试题之类的重要思想就是，找对公式

动态规划也是如此，找不到公式就写不出来

n的阶乘f(n) = n*f（n-1）

除了公式，还有设置结束条件



算阶乘：

```c
int f(int n) {
	if (n == 1) {
		return 1;
	}
	return n * f(n - 1);//找公式
}
int main() {
	int n;
	while (scanf("%d", &n) != EOF) {
		printf("!n = %d\n", f(n));
	}
	system("pause");
	return 0;
}
```



上台阶：

![image-20221120164950011](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120164950011.png)

f(n) = f(n-1) +f(n-2)

上台阶的思维要从终点来看。

走到n个台阶这个位置，一次只能1或2步，那么你能来到今天这个位置，

要么就是从倒数第一个台阶走1步上来的，要么就是从倒数第二个台阶跨2步上来的

而对于f(n-1)和f(n-2)来说，也是把他们各自视为终点，再往前-1或-2

故最终公式就是

f(n) = f(n-1) +f(n-2)

依次类推，如果你可以走1或2或3个台阶时，就是

f(n) = f(n-1) +f(n-2)+f(n-3)

数学上的本质就是斐波契那函数

```c
int step(int n) {
	if (1 == n) {
		return 1;//第一阶时，回到地面只有1种走法
	}
	if (2 == n) {
		return 2;//第二台阶时，回到地面2种走法
	}
	return step(n - 1) + step(n - 2);//step的参数取值1，2时必须返回了
}
int main() {
	int n;
	while (scanf("%d", &n) != EOF) {
		printf("n个台阶的走法  %d\n", step(n));
	}
	system("pause");
	return 0;
}
```





汉诺塔：

![image-20221120170806095](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120170806095.png)



![image-20221120170812926](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120170812926.png)

```c
void move(char x, char y) {
	printf("%c --> %c\n", x, y);
}

void hanoi(int n, char one, char two, char three) {
	if (n == 1) {
		move(one, three);
	}
	else {
		hanoi(n - 1, one, three, two);		//这是关键
		move(one, three);					//这就是最大那个盘子，从1柱移到3柱
		hanoi(n - 1, two, one, three);	//剩余的n-1个盘子，本来在2柱了，借助1柱，转移到3柱
	}
}
int main() {
	int n;
	while (scanf("%d", &n) != EOF) {
		printf("n个盘子的汉诺塔玩法 \n");
		hanoi(n,'A','B','C');
	}
	system("pause");
	return 0;
}
```





递归题就是要站在最后想，n和n-1，n-2，n-3之类的关系





# 变量及函数作用域

## 程序块

![image-20221120173442142](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120173442142.png)

（4）最容易出错，程序块，java中也有

局部变量，又称内部变量在离他最近的大括号内定义的有效

注意了，![image-20221120173738388](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120173738388.png)

这样也是错的，{}内自成一方小天地



## extern



![image-20221120173813918](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120173813918.png)

全局变量不是在整个文件中都有效，不是的，在他定义之前，之上的地方，是无效的

从定义变量位置开始到本源文件结束，又称全程变量

如果要写一个所有源文件都可以用的变量，

加上extern就会扩展变量的作用域，可以在别的源文件中用其他源文件里的extern变量

要借用的源文件要写extern，而不是最初定义的要加。

不可以把全局变量写在头文件里

头文件里只能写引用的公共头文件，以及结构体类型的定义



然而，有时候，你不想让别的源文件随便一个extern就能借用你的全局变量，改完了改坏了把麻烦事情扔给你。

这又引出了静态变量的存在



## static

![image-20221120175335559](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120175335559.png)



如果你在你的全局变量中static了一个变量，外界就无法extern你的了

static后该变量只对本文件可见。不对外暴露。

故，这个时候，外面的文件也可以定义你这个变量名，而不用担心重名。



函数的默认前缀是extern的，于是你可以在main.c中调用func.c的函数而只需要声明

倘若在函数前面加上static ，就和修饰变量一样，main.c就无法调用func.c中static的函数了



static修饰全局变量	或者函数时，很好理解，就是屏蔽外界的，仅本文件可见



而static修饰局部变量时，就和上文不一样了

static修饰局部变量主要起到，将变量保存在静态区，不会清零，不会释放，！只会初始化1次！

静态区和全局区混在一起

但

每个函数都能static它的局部变量，不用担心重名，每个函数都可以static int times = 0；

这就是静态局部变量的用处。

要与静态全局变量区分开来。



## register

![image-20221120181713427](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120181713427.png)

就是常用的，扔寄存器里存着。

寄存器变量往往和for循环挂钩，把某个变量扔寄存器里，保持良好性能，就少了一次，从内存到寄存器的过程



# 函数调用原理

具体Linux阶段再讲

函数调用原理：

![image-20221120181826738](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120181826738.png)

![image-20221120181835527](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120181835527.png)![image-20221120181841957](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120181841957.png)

![image-20221120181854168](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221120181854168.png)
