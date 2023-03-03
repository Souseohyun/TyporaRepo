## 链表概念



![image-20221121121354133](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121121354133.png)

## 链表的定义，基本结构

![image-20221121121425855](C:\Users\23614\AppData\Roaming\Typora\typora-user-images\image-20221121121425855.png)

可以把结构体定义等扔在单独的头文件里

把相关增删查改的函数写在fun.c的文件里

方便复用



一般会弄个指针指向链表尾部，不然每一次都得遍历找到尾部进行操作，很麻烦



## 打印/插入



头插法------尾插法-------有序插入



打印链表：

```c
//打印链表
void list_print(pstu phead) {//一级指针，值传递，不改变实参的指向
	while (phead) {
		printf("%3d", phead->num);
		phead = phead->pNext;
	}
	printf("\n");
}
```





头插法：(纯C)

pstu为结构体指针，pphead，pptail为二级指针

```c
//头插法
void list_head_insert(pstu* pphead, pstu* pptail, int val) {
	pstu pnew = (pstu)malloc(sizeof(stu));
	pnew->num = val;
	pnew->pNext = NULL;
	//判断链表是否为空
	if (NULL == *pphead) {			//pphead指向一级指针的地址，*pphead就是子函数的phead的值
		//链表为空的时候，头尾指针指向第一个节点
		*pphead = pnew;
		*pptail = pnew;
	}
	else {
		//把原有的头指针复制给新指针的pNext
		pnew->pNext = *pphead;
		*pphead = pnew;

	}
}
```





尾插法：(纯C)

```c
void list_tail_insert(pstu* pphead, pstu* pptail, int val) {
	pstu pnew = (pstu)calloc(1,sizeof(stu));//calloc与malloc区别
	pnew->num = val;
	//calloc申请的空余的为00，不需要pNext置NULL
	if (NULL == *pphead) {//头节点为空，链表是空的
		*pphead = pnew;
		*pptail = pnew;
	}
	else {
		(*pptail)->pNext = pnew;		//二级指针解引用，相当于子函数中ptail->pNext
		*pptail = pnew;					//更新尾结点
	}

}
```





顺序插入：

```c
//有序插入
void list_sort_insert(pstu* pphead, pstu* pptail, int val) {
	pstu pnew = (pstu)calloc(1, sizeof(stu));
	pnew->num = val;
	if (NULL == *pphead) {
		*pphead = pnew;
		*pptail = pnew;
	}
	else if (val < (*pphead)->num) {//插到头部
		pnew->pNext = *pphead;
		*pphead = pnew;
	}
	else if (val > (*pptail)->num) {//插到尾部
		(*pptail)->pNext = pnew;
		*pptail = pnew;
	}
	else {			//插到中间需要遍历
		pstu pcur = *pphead;//工作指针，用来遍历，总不能头尾指针移动，会影响外面
		pstu pre = NULL;//pre,由于链表没有指向上一级的指针，所以要时刻记录工作指针的上一个，用来插入
		while (pcur) {//工作指针指向null时
			if (val < pcur->num) {//要插入的值，比工作指针指向的结点值要小时，可以插到工作指针指向结点前面
				pnew->pNext = pcur;//或者=pre->pNext
				pre->pNext = pnew;
				break;
			}
			//pcur++;错误写法，链表的内存不一定连续，++无法做到
			pre = pcur;//pcur下一步之前，pre记录一下.
			pcur = pcur->pNext;
		}
	}

}
```





## 删除

按值删除

```c
//按值删除
void list_val_delete(pstu* pphead, pstu* pptail, int val) {
	pstu pcur = *pphead;
	if (pcur == NULL) {//这里很重要，如果没有判空，下四行中pcur->num时空指针拿数据会报错
		printf("list is empty\n");
		return 0;
	}
	if (val == pcur->num) {//如果是第一个结点，需要特殊处理头指针
		*pphead = pcur->pNext;
		free(pcur);
		pcur = NULL;
	}
	else {
		pstu pre = *pphead;
		while (pcur) {
			if (pcur->num == val) {
				pre->pNext = pcur->pNext;//越过当前结点
				if (val == (*pptail)->num) {//如果删除的是尾结点，特殊处理pptail
					*pptail = pre;
				}
				free(pcur);
				pcur = NULL;
				return;
			}
			pre = pcur;
			pcur = pcur->pNext;
		}
		printf("this val is not in list\n");

	}
}
```



## 修改

修改节点数据不需要更改结点指向，所以可以用一级指针传参。

```c
//修改
void modify(pstu phead, int num, float score) {
	while (phead) {
		if (phead->num == num) {
			phead->score = score;
			return;
		}
		phead = phead->pNext;
	}
	printf("修改失败，没有找到该学号\n");
}
```

















