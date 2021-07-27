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



