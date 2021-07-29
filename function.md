[toc]

# 函数

## 一、函数的定义

> 数据类型	函数名	([数据类型 形参名，数据类型 形参名])

>  **主函数main**

1. 无参数形式

```c
int main( void )
{
    ...
    return 0;
}
```

2. 带参数形式

```c
int main( int argc, char *argv[] ) 
{
    ...
    return 0;
}
// 第一个参数是命令行中的字符串数	argument count
//第二个参数是一个指向字符串的指针数组 argument value
```

```c
#include<stdio.h>
#include<stdlib.h>

int main(int argc,char *argv[])
{

        int i;
        printf("%d\n",argc);


        for(i = 0;argv[i] != NULL;i++)
                puts(argv[i]);                                                  
        return 0;
}

/*
命令行输入./main ls man

输出为
3
./main
ls
man

*/
```



## 二、函数的传参

1. 值传递

2. 地址传递

```c
#include<stdio.h>
#include<stdlib.h>


int print_value(int a,int b)    //  只能这样写，不可写成int a,b，函数调用完释放
{

        printf("%d %d\n",a,b);      //按顺序传参
        return 0;
}

void swap(int *p1,int *p2)
{
        int tmp;
        tmp = *p1;
        *p1 = *p2;
        *p2 = tmp;

}

int main()
{

        int i = 3,j = 5;    //i,j实际参数

        print_value(i,j);		// 值传递
        swap(&i,&j);             //地址传递，可通过swap函数改变mian函数里的值。                            
        print_value(i,j);
        exit(0);
}
```

3. 全局变量

## 三、函数的调用

>  当被调函数写在main函数下方时，应先在main函数上面声明函数；如`void print_value();`

### １. 嵌套调用

```c
// 嵌套调用实现比较三个数中最大值与最小值的差；
#include<stdio.h>
#include<stdlib.h>

int max(int a,int b,int c)
{
        int tmp;
        tmp = a > b ? a : b;
        return tmp > c ? tmp : c;
}

int min(int a,int b,int c)
{
        int tmp;
        tmp = a < b ? a : b;
        return tmp < c ? tmp : c;

}

int dist(int a,int b,int c)
{
    return max(a,b,c) - min(a,b,c);
}
int main()
{

        int a = 9,b = 4,c = 2;
        int max1,min1,res;                  //不能定义与函数名相同                                    
        
        max1 = max(a,b,c);
        min1 = min(a,b,c);
        res = dist(a,b,c);

        printf("%d - %d = %d",max1,min1,res);
        return 0;
}
// 9 - 2 = 7
```

### 2. 递归

> 函数直接或间接调用自身

1.  阶乘

```c
# include<stdio.h>

int recursion(int n)
{
        if(n < 0)
                return -1;
        if(n == 1 || n == 0)
                return 1;  
        return n*recursion(n -1); //
}

int main()
{

        //n! = n * (n -1)!
        //          (n - 1)! = (n - 1) * (n -2)!
        //                      

        int num,res;
        
        printf("请输入要阶乘的数：");
        scanf("%d",&num);

        res = recursion(num);
        printf("%d! = %d\n",num,res);
                                                                                
        return 0;
}
```

2. 斐波那契

```c
# include<stdio.h>

//1,1,2,3,5,8,13,21,34.....
int fibo(int n)
{


        if (n == 1 || n == 2)
                return 1;
        
        return fibo(n -1) + fibo(n -2);
}


int main()
{

        int n,res,i = 1;
        printf("打印fibonacciq前几项：");
        scanf("%d",&n);

        while(n > 0)
        {

        res = fibo(i);                                                          
        printf(" %d",res);
        i++;
        n--;
        }
        printf("\n");
        return 0;
}
```

## 四、函数与数组

### 1. 一维数组

```c
#include<stdio.h>
#include<stdlib.h>

void print_arr(int *p,int n)//接受起始地址与元素个数
//void print_arr(int p[],int n)　//形参中p[]等价于一个指针int*
{
        int i;
        printf("%s:%d\n",__FUNCTION__,sizeof(p));//只能知道指针p占用的字节，得不
到数组字节
        for(i = 0;i < n;i++)
                printf("%d ",*(p+i));
            //  printf("%d ",p[i]);                                             
        printf("\n");
}

int main()
{

        int a[] = {1,4,2,5,7};

        printf("%s:%d\n",__FUNCTION__,sizeof(a));//查看数组字节
        print_arr(a,sizeof(a)/sizeof(*a)); //数组传参时要传递数组的个数

        exit(0);
}
//main:20
print_arr:8
1 4 2 5 7 
    
/*int main(int argc,char **argv)
	int main(int argc,char *argv[])
```

```c
int a[n] = {};		
int *p = a;
```

| 传参 | a    | *a   | a[0] | &a[3] | p[1] | *p   | p    | p+1  |
| ---- | ---- | ---- | ---- | ----- | ---- | ---- | ---- | ---- |
| 接收 | int* | int  | int  | int*  | int  | int  | int* | int* |

### 2.　二维数组

```c
# include<stdio.h>                                                              
# include<stdlib.h>

#define M 3
#define N 4
void print_arr(int *p,int n)
{
        int i;
        for(i = 0;i < n;i++)
        {
                printf("%4d",p[i]);
        }
        printf("\n");
}

void print_arr1(int (*p)[N],int m,int n)
        //      int p[][N]      形参中等价于数组指针，占８字节
{

        int i,j;
        for(i = 0;i < m; i++)
        {
                for(j = 0;j < n;j++)
                        printf("%4d",*(*(p + i)+j));
                        //              p[i][j]
                printf("\n");
        }
        printf("\n");
}
int main()
{


        int i,j;
        int a[M][N] = {1,2,3,4,5,6,7,8,9,10,11,12};

        print_arr(&a[0][0],M*N);// *a   a[0]    *(a+0)传首地址和元素个数
        print_arr1(a,M,N);
        exit(0);
}
```

```c
int a[M][N] = {...};
int *p = *a;
int (*q)[N] = a;
```

| 传参 | a[i] [j] | *(a+i)+j | a[i]+j | p[i] | *p   | q[i] [j] | *q   | q           | p+3  | q+2        |
| ---- | -------- | -------- | ------ | ---- | ---- | -------- | ---- | ----------- | ---- | ---------- |
| 接收 | int      | int*     | int*   | int  | int  | int      | int* | int (**[N]) | int* | int (*)[N] |

> n维数组a，数组名a指向a[0]，a[0]是n-1维数组，数组名a[0]指向a[0][0]，a[0][0]是n-2维数组.......
>
> 数组名总是和 &数组名[0] 等价(值、类型)

|  |         | p的类型 |
| :----------------: | :-------: | ------- |
|  int (*p) [Ｍ]　[N] |          &a     |     int ***     |
|    int (*p) [N]    |     a     | int **  |
|    int (*p) [N]    |   &a[0]   | int **  |
|       int *p       |  a[0]/*a  | int *   |
|       int *p       | &a[0] [0] | int *   |
|       int p        | a[0] [0]  | int     |


```c
//test
# include<stdio.h>
# include<stdlib.h>


int main()
{

        int a[3][4] = {1,2,3,4,5,6,7,8,9,10,11,12};

        int *p1 = *a;					//值均为a[0][0]地址
        int *p2 = a[0];
        int (*p3) [3][4] = &a;
        int (*p4) [4] = a;
        int (*p5) [4] = &a[0];

        printf("%p %d\n",p1,sizeof(p1));				//0x7fff0a220a50	 8	
        printf("%p %d\n",p2,sizeof(p2));
    
        printf("%p %d\n",p3,sizeof(p3));
    
        printf("%p %d\n",p4,sizeof(p4));
        printf("%p %d\n",p5,sizeof(p5));
        printf("\n");


        printf("%p %d\n",p1+1,*(p1+1));			//0x7fff0a220a54  2
        printf("%p %d\n",p2+1,*(p2+1));			//0x7fff0a220a54  2
    
        printf("%p %p\n",p3+1,p3);                	//0x7fff0a220a80  0x7fff0a220a50
    
        printf("%p %d\n",p4+1,*(*(p4+1)));			//0x7fff0a220a60  5
        printf("%p %d\n",p5+1,*(*(p5+1)));			//0x7fff0a220a60  5
        exit(0);
}
```

### 3. 字符数组

```c
# include<stdio.h>
# include<stdlib.h>

char *mystrcpy(char *dest,const char *src)//返回dest的起始位置
{

        char *ret = dest;   
        if(dest != NULL && src != NULL)
                while((*dest++ = *src++) != '\0');

        return ret;
}

int main()
{


        char str1[] = "beiqinb!";
        char str2[128];
        char *ret;

        printf("%p\n",str2);
        ret = mystrcpy(str2,str1);                                              
        puts(str2);
        printf("%p",ret);

        exit(0);
}
```

## 五、函数与指针

### 1. 指针函数

> 一个函数，返回值为指针	`int *fun(形参);`	`int *fun(int a,int b)`

### 2. 函数指针

> 一个指向函数的指针	`int (*P)(形参);`	`int (*P)(int ,int)`

```c
# include<stdio.h>
# include<stdlib.h>

int add(int a,int b)
{
        return a+b;
}

int main()
{

        int a = 1,b = 88;
        int ret;
        int (*p)(int,int);//形参名可省略                                        

        p = add;
        ret = p(a,b);
        printf("%d\n",ret);

        exit(0);
}
```

### 3. 函数指针数组

> 存放函数指针的数组	`int (*arr[N](形参))`	`int (*arr[N])(int)`

```c
# include<stdio.h>
# include<stdlib.h>

int add(int a,int b)
{
        return a+b;
}

int sub(int a,int b)
{
        return a-b;
}
int main()
{

        int a = 1,b = 88;
        int ret,i;

        int (*p[2])(int,int);


        p[0] = add;
        p[1] = sub;                                                             
        for(i = 0;i < 2;i++)
        {   
                ret = p[i](a,b);
                printf("%d\n",ret);
        }
        exit(0);
}
```

