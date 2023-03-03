# day2总结



![image-20221108144837244](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221108144837244.png)

新手犯错1：如果发现输入一次回车scanf不会结束阻塞，尝试检查你的scanf函数中是否被你塞了\n进去



# 选择结构

自动化格式代码：

ctrl+k ctrl+f



if代码略过，感觉已经这个都很熟悉了



## switch

回忆一下，感觉switch用的很少



switch（）里面只能填整形或者字符型的，绝对不可以填浮点数，填浮点数，下面的case就匹配不上

（原因是浮点数比较相等时，有些等不了的那个问题）

选择项有case 和default：

容易忘记：每一个case项要记得break；当然也可以利用这个点来把共同点归类输出





# 循环结构

## goto语句

goto需要谨慎使用

goto		只能在一个函数内使用

向下跳转主要可以跳过一些代码段。

![image-20221108153421292](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221108153421292.png)



## while

先判断，再进入循环

在Windows下我们可以用fflush，rewind清空缓冲区，但Linux下没有这些，可以手写一个代替		

``` c
while((ch = getchar()) != EOF && ch != '\n');
```



## do while

先执行循环体，后判断循环条件

注意格式，该格式中while（条件）后一定要记得加  ;



do while 与continue：

![image-20221108160348979](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221108160348979.png)



continue 只会跳到do对应的大括号，不会跳过判断条件















