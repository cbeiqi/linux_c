[toc]



# point指针

> 书写 int *p = &a;
>
> 理解 int* p = &a;

`*&a`可以理解为`*(&a)`，`&a`表示取变量 a 的地址，`*(&a)`表示取这个地址上的数据，绕来绕去，又回到了原点，`*&a`仍然等价于 a。

> int ***p3 = &p2;

以三级指针 p3 为例来分析上面的代码。`***p3`等价于`*(*(*p3))`。*p3 得到的是 p2 的值，也即 p1 的地址；*(*p3) 得到的是 p1 的值，也即 a 的地址；经过三次“取值”操作后，*(*(*p3)) 得到的才是 a 的值。



## 1. 变量与地址	指针与指针变量	指针访问

```c
#include<stdio.h>
#include<stdlib.h>

int main()
{

        int i = 1;
        int *p = &i;
        
        int *a;
        double *b;
        char *c;			// 同一平台占用字节相同，不同类型读取方式不同；

        printf("i的值为%d\n",i);
        printf("i的地址为%p\n",&i);
        printf("p的地址为%p\n",&p);
        printf("p的值为%p\n",p);
        printf("*p的值为%d\n",*p);


        printf("%d\n",sizeof(p));
        printf("%d\n",sizeof(a));
        printf("%d\n",sizeof(b));
        printf("%d\n",sizeof(c));                                               
        exit(0);
}

/*i的值为1
i的地址为0x7ffc609ca45c
p的地址为0x7ffc609ca460
p的值为0x7ffc609ca45c
*p的值为1
8
8
8
8	//64位占用8
*/

// 直接访问：通过&i访问i
// 间接访问: 通过指针访问

 int a =1;
int *p1 = &a;
int **p2 = &p1;

// i	*p1	**p2
// &i	p1	*p2		访问方式

```

## 2. 空指针，野指针

> int *p;
>
> 指针变量没有被初始化，指向一个非法的或已销毁的内存的指针，对系统造成不可预知的错误;
>
> 为了避免此类野指针的出现，指针变量在创建的同时应该被初始化，要么将它设置为NULL;
>
> int *p = NULL;

## 3. 空类型指针

`void *p = NULL`

而空类型的指针是有指向的指针，但它的指向的数据的类型暂时不确定，所以先弄成void * 类型，后期再进行转换

## 4. 指针运算

> & * 关系运算	++	--

## 5. 指针与数组

### 指针与一维数组

数组：连续存放。数组的取值方式`a[i] = *(a + i)`，a表示a[0]的地址，即数组的起始地址

实际上系统在编译时，数组元素 a[i] 就是按 *(a+i) 处理的。即首先通过数组名 a 找到数组的首地址，然后首地址再加上i就是元素 a[i] 的地址，然后通过该地址找到该单元中的内容.

> TYPE NAME = VALUE;
> a[i] = a[i] = * (a+i) = *(p+1) = p[i];
> &a[i] = &a[i] = a+i = p+i = &(p[i]);

```c
# include<stdio.h>
# include<stdlib.h>


int main()
{


        int a[3] = {1,2,3};
        int *p = a;
        int i;

        for(i=0;i< sizeof(a)/sizeof(*a);i++)
                printf("%p -> %d\n",a+i,a[i]);

        for(i=0;i< sizeof(a)/sizeof(*a);i++)
                printf("%p -> %d\n",p+i,a[i]);

        for(i=0;i< sizeof(a)/sizeof(*a);i++)
                printf("%p -> %d\n",p+i,*(p+i));
        
        for(i=0;i< sizeof(a)/sizeof(*a);i++)
                printf("%p -> %d\n",p+i,p[i]);    
		
    	exit(0);
}
/*
0x7ffcdd71cedc -> 1
0x7ffcdd71cee0 -> 2
0x7ffcdd71cee4 -> 3
0x7ffcdd71cedc -> 1
0x7ffcdd71cee0 -> 2
0x7ffcdd71cee4 -> 3
0x7ffcdd71cedc -> 1
0x7ffcdd71cee0 -> 2
0x7ffcdd71cee4 -> 3
0x7ffcdd71cedc -> 1
0x7ffcdd71cee0 -> 2
0x7ffcdd71cee4 -> 3
*/
```

> ==p+1 的本质是移到数组下一个元素的地址==，所以关键是看数组是什么类型的。比如数组是 char 型的，每个元素都占一字节，那么此时 p+1 就表示往后移一个地址。所以不同类型的数组，p+1 移动的地址的个数是不同的，但都是==移向下一个元素==。

```c
// 通过p引用数组当中的元素，可以不用数组名
int *p = (int [3]){1,2,3,};
```

```c
int a[] = {3,5,1}
int y;
int *p = &a[1];

y = (*--p)++;
// y = 3			a[0] = 4
```

### 指针与二维数组

> 在内存中，二维数组的分布是==一维线性==的，整个数组占用一块连续的内存
>
> `p[i][j] = *(*(p + i) + j)`

> `*(p+1)`单独使用时表示的是第 1 行数据，放在表达式中会被转换为第 1 行数据的首地址，也就是第 1 行第 0 个元素的地址，因为使用整行数据没有实际的含义，编译器遇到这种情况都会转换为指向该行第 0 个元素的指针；就像一维数组的名字，在定义时或者和 sizeof、& 一起使用时才表示整个数组，出现在表达式中就会被转换为指向数组第 0 个元素的指针。

> `*(p+1)`表示取地址上的数据，也就是==整个第 1 行数据==，是多个数据，不是第 1 行中的第 0 个元素，为行指针

```c
int a[3][3] = {{1,2,3,},{4,5,6},{7,8,9}};

printf("%p\n",*(a + 1));		//0x7ffe1c1556fc

printf("%d\n",**(a+1));			//4
printf("%d\n",*(*(a+1)+2));  		//6

printf("%d\n",sizeof(*(a + 1)));		//12

int *p;
// p = a;		报警告，a为行指针，p为列指针。
p = & a[0][0];  // 或*a
```

### 指针与字符数组

```c
char *str;
str = "hello";
//除了字符数组，C语言还支持另外一种表示字符串的方法，就是直接使用一个指针指向字符串
char *str = "hello";
```

#### 字符数组与字符指针

`char *str = "This is a string.";`

是对字符指针进行初始化。此时，字符指针指向的是一个字符串常量的首地址，即指向字符串的首地址。*str*是一个变量，可以改变*str*使它指向不同的字符串，但==不能改变==*str*所指的字符串常量。

如`strcpy(str,"world")`会报段错误，因为你试图使用字符串world覆盖字符串常量hello.



`char string[ ]="This is a string.";`

此时，*string*是字符数组，它存放了一个字符串。*string*是一个数组，==可以改变==数组中保存的内容。

> 注意：str是一个局部指针变量，存储在栈中，而“hello”则是存储在常量区的。
>
> 但是，string以及其中的数组，hello是存储在栈中的。

## const与指针

### 指针常量

`int *const p;` const修饰p	p指向的地址不能改变；但可以改变*p的值

```c
int i=1;
int j = 2;
int *const p = &i;

 // p = &j;		F  assignment of read-only variable ‘p’
printf("%p\n",p);		//0x7ffcb61c4648
*p = 2;						
printf("%p\n",p);				//0x7ffcb61c4648
 printf("%d\n",i);           //2	通过*p修改i成功
```

### 常量指针

`int const *p;`或`const int *p;`const 修饰*p	*p的值不能改变	但p指向的地址可以改变，即p可以改变

```c
int i=1;
int j = 2;
int const *p = &i;

// *p = 2;		F	assignment of read-only location ‘*p’
printf("%p\n",p);	//0x7fffaa3764d8
p = &j;	
printf("%p\n",&j);		//0x7fffaa3764dc
 printf("%p\n",p);  		//0x7fffaa3764dc	改变p的指向成功
```



另外，const修饰一般变量，不能通过变量名修改num的内容，可通过num的地址修改。

```c
const int num = 100;
int* p = &num;
num = 200;//用这种方式修改变量会报错，走不通。
*p = 200;//这种方式是可行的。
```



## 数组指针，指针数组

### 数组指针

> 指向一个数组的指针
>
> 【存储类型】 数据类型	（*指针名） 【下标】 = 值；

定义一个二维数组指针

> `int (*p)[3]`--> type name : int[3] *p;	p+1时移动3个元素的大小；	

```c
int a[3][3] = {{1,2,3,},{4,5,6},{7,8,9}};
int (*q)[3];
q = a;
```

### 指针数组

> 一个数组中的所有元素保存的都是指针，称它为指针数组。除了每个元素的数据类型不同，指针数组和普通数组在其他方面都是一样的。
>
> 【存储类型】 数据类型	*指针名【下标】；

```c
#include <stdio.h>

int main()
{
    int a = 1, b = 2, c = 10;
    
    //定义一个指针数组
    int *arr[3] = {&a, &b, &c};//也可以不指定长度，直接写作 int *parr[]
    
    //定义一个指向指针数组的指针
    int **parr = arr;
    printf("%d, %d, %d\n", *arr[0], *arr[1], *arr[2]);
    printf("%d, %d, %d\n", **(parr+0), **(parr+1), **(parr+2));
    // 1,2,10
    exit (0);
}
```

字符数组排序

```c
// 按首字母排序

# include<stdio.h>
# include<stdlib.h>
# include<string.h>

int main()
{


        int i,j,k;
        char *tmp;
        char *str[5] = {"Beiqi","Sarcher","Hebe","Ella","Bou"};


        for(i = 0;i<5;i++)
                puts(str[i]);

        printf("\n");
        //选择法排序

        for(i=0;i<5-1;i++)
        {
                k = i;
                for(j=i+1;j<5;j++)
                {
                        if(strcmp(str[k],str[j]) > 0)
                                k = j;
                }
                if(k != i)
                {
                        tmp = str[i];
                        str[i] = str[k];
                        str[k] = tmp;
                }
        }
        for(i = 0;i<5;i++)
                puts(str[i]);

        exit(0);
}   
```

## 多级指针



