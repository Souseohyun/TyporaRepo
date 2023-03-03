# 二维数组

二维数组不存在所谓的二维的存储结构，他仍然是在内存空间中，也是一串连续的1010

![image-20221111102325485](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111102325485.png)

![image-20221111104259437](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111104259437.png)

sizeof数组会得到完整的数组占用栈空间内存大小。

## 二维数组的初始化

![image-20221111102414281](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111102414281.png)

没初始化，只声明，会给内存写出cccc

只有初始化操作了，才会默认给你后面附加\0



## 打印二维数组

![image-20221111105120545](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111105120545.png)



# 二维数组在传递时

一维数组的数组名里存的是一个整形指针，二维数组的里面存的是一个整形数组指针。



众所周知，一维数组做参数传递时会退化成指针，所以形参只需要int arr[]

而二维数组，特殊的一点在于，他能把列传进去，原因见指针章节，

所以我们形参的列的参数要和实参的保持一致，并传入“传不进的行row”

``` c
void two_arr_print(int arr[][4],int row) {

}
```

![image-20221111104553424](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111104553424.png)

此时sizeof只会得到一个普通的指针的内存空间，那就是4；

***注意了，此时再sizeof(arr[0])反而会得到16（一行4列，4x4=16）***

![image-20221111104903527](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111104903527.png)



arr[0]反而正常的成为了一个一维数组，被传入。  

![image-20221111105117063](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111105117063.png)







# **字符数组

![image-20221111105748867](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111105748867.png)

tips1：char型变量要用单引号引起来。

tips2：char c[10] = {1,2,3,4,5};这样是不会输出数字的12345的，是ASCII码所对应的。



关于初始化：

tips3：一般人们也不会char c[6] = {'h','e','l','l','o'};来完成初始化，过于繁琐了，

大家一般用char c[6] = "hello"; 		注意哦，这里一定是6，要给字符串尾标识符\0存储的地方哦



关于输出：

tips4：使用for循环之类的一个个打印char型数据实在太麻烦，一般用%s来输出，%s识别内存中00而结束，也就是ascii码的/0



## 自己写输出

``` c
void char_arr_print(char c[]) {
	int i = 0;
	while (c[i]) {
		printf("%c", c[i]);
		i++;
	}
	printf("\n");
}
```

while中只需要写c[i]就行，到了\0,也就是ascii码对应的0，也就是内存中的00时，自然会结束。





## 输入

![image-20221111164526187](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111164526187.png)

常用scanf()搭配%s来进行对字符串的输入，但需要注意的是

有且只有%c地位非凡，可以接受空格回车制表符，%s也不能僭越

于是输入how     are you

首先输出how，内存中在how后面就是自主添加的\0

第一个scanf函数就这样结束了，%s吃不掉空格，遇到了就结束了该函数

第二个scanf函数就面临着        are you的情况，are前面有多个空格

但就像scanf函数开始接收参数时，只要不是%c，就会无视掉前面的空格，程序指针pc将无情的滑过一长串空格，直到指向are



![image-20221111164803626](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111164803626.png)





注，空格与回车同样是会结束scanf对已接收数据的阻塞状态，并留存在stdin中



![image-20221111182407273](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111182407273.png)

![image-20221111182546590](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111182546590.png)



也就是说，scanf("%s")是不能读取完整的一行的，只能读一个字符串

如果你想类似学生管理系统一次输入多个单词

得用多个%s，中间可以不加空格，因为%s遇到空格会停止接收。

![image-20221111192141194](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111192141194.png)



# gets读取一行

gets函数是整行整行的。

注意！！fgets将把你输入的\n也一起写入数组中。！！！注意

gets不会。scanf加%s也不会。

gets读到\n取出并丢弃，fgets会tm给你存进来。

scanf加%s会把回车符扔在stdin中不管。

![image-20221111192419712](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111192419712.png)

gets()的返回值是char *型所以不能返回EOF，故不能使用

while(gets()!=EOF){}；

得用！=NULL

gets()的基本用法，就是踏踏实实的读取一行，受回车影响，不受空格

![image-20221111193830600](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111193830600.png)

注意：

 fgets() 和 gets() 一样，最后的回车都会从缓冲区中取出来。

只不过 gets() 是取出来丢掉，而 fgets() 是取出来自己留着。但总之缓冲区中是没有回车了！

所以与 gets() 一样，在使用 fgets() 的时候，如果后面要从键盘给字符变量赋值，那么同样不需要清空缓冲区。

如果搭配puts这种输出完后会自动给你换行的函数，就会连换俩行。。

![image-20221118105631462](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118105631462.png)

同样的，一次ctrl+z就可以终止该while了。



# puts读出一行

puts默认在输出的一行后加换行。



# str系列

## strlen()

当你想统计字符串的长度时，

使用sizeof(arr)：

![image-20221111195425825](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111195425825.png)

sizeof arr 将会统计arr这个数组在栈空间所占有的全部内存，用于是100



而strlen()显然更智能，依靠\0来界定字符串的长度，统计包括空格

![image-20221111195754234](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111195754234.png)

自己写strlen()

![image-20221111200253000](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111200253000.png)



## strcpy()

strcpy()中，前面是目的数组，后面是源数组。

![image-20221111202444565](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111202444565.png)





## strcmp()

strcmp ---->  str  Compare

比较俩字符数组

比较规则：

如果俩字符数组一模一样，返回0

如果前者大于后者，则返回大于0的数

小于则反之。

如何比较大小呢，及比较ascii码，从首位开始，相同就下一位

![image-20221111203212103](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111203212103.png)

比较h相同，比较e和o，e在o前面，故o大，how更大，drr更大，strcmp(arr,drr)小于0



strcmp是能用来比较字符数组，不要随便用来比较整形之类的数组。

![image-20221118140418439](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221118140418439.png)

按照比较字符串的规则，比到00就结束了，一个个字节的比，是不行的。

这个时候可以用memcpy(a,b,sizeof(a));



## strcat()

拼接字符串。

strcat(arr,drr);

把drr拼接到arr后面，

arr的\0会被覆盖，cat()会给你自动添\0

例如arr是man

drr是woman

这俩拼接最后需要3+5+1=9个格子，9个字节，char占1字节

![image-20221111203648647](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221111203648647.png)









## strrev()

strrev已经被弃用了，要用新版本_strrev()

逆转字符数组，堆空间的字符串也行，总之要让人家能改。







