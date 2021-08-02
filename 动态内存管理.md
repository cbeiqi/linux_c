[toc]

# 动态内存管理

## 1. 一些函数

> 原则：谁申请谁释放

**size_t** 类型表示C中任何对象所能达到的最大长度，它是无符号整数

```c
malloc();	calloc();	realloc();	free();
       void *malloc(size_t size);//分配size空间
       void free(void *ptr);//free() 不会改变 ptr 变量本身的值，调用 free() 后它仍然会指向相同的内存空间，内存释放只是告诉OS该内存区域不再被本进程独占，可以被其他进程所使用。所以最好在free之后，使指针指向NULL。
       void *calloc(size_t nmemb, size_t size);//申请一块存放n和memb的空间(n*size)
       void *realloc(void *ptr, size_t size);//重新分配空间
```

```c
#include<stdio.h>
#include<stdlib.h>

int main()
{

        int *p =NULL;

        p = malloc(sizeof(int));//申请一块int大小的空间，malloc返回指向这块地址>的指针，用一个指针接收;
        if(p == NULL)
        {       
            printf("malloc error\n");
            exit(1);
        }
        *p = 10;
        printf("%d\n",*p);

        free(p);    //释放p这块空间                                             
        exit(0);
}
//变长数组
#include<stdio.h>
#include<stdlib.h>

int main()
{

        int *p;
        int i;
        int num = 5;

        p = malloc(sizeof(int) * num);
        for(i = 0;i < num;i++)
                scanf("%d",p+i);
        for(i = 0;i < num;i++)
                printf("%d\t",*(p+i));
        printf("\n");
        free(p);                                                                
        exit(0);
}
```

## 2、typedef

> 为已有的数据类型改名

`typedef int INT;`-->将int 改名为INT，方便移植

```c
//与define的区别
#define IP int *
IP p,q;		--> 	int *p,q;
typedef int *IP;
IP p,q; 	--> 	int *p,*q;
```

```c
//为数组改名		直接眯着眼用a代替ARR
typedef int ARR[6];
ARR a; 	--> 	int a[6];
//为结构体改名
struct node_st
{
    int i;
    float f;
};

typedef struct node_st NODE;
typedef struct node_st *NODEP;
//以上等价与
typedef sttruct
{
    int i;
    float f;
}NODE,*NODEP;
//
NODE a; 	--> 	struct node_st a;
NODE *p; 	--> 	struct onde_st *p;

NODEP p; 	--> 	struct node_se *p;
//为函数改名
typedef int FUNC(int);
FUNC a; 	--> 	inf a(int);
    
typedef int *FUNCP(int);
FUNCP p; 	--> 	int *p(int);
//为函数指针改名
typedef int *(* PFUNC)(int);	//指向指针函数的函数指针
PFUNC pf; 	--> 	int *(*p)(int)
```











