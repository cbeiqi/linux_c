[toc]



# 数组array

> **构造类型之一，连续存放**





## 一维数组

### 1. 定义

> （存储类型)	数据类型	标识符号[下标];

### 2.初始化

1. 不初始化－不确定
2. 全部初始化
3. 部分初始化－按顺序初始化，没初始化的部分为０
4. **若存储类型设为`static`未初始化的都为０**

```c
# include <stdio.h>                                                             
# include <stdlib.h>

# define M 4
int main()
{

        int i;
        int arr0[] = {1,3};	//按初始化分配空间
        int arr1[M];
        int arr2[M] = {1,2,3,4};                                                
        int arr3[M] = {1,3};

        for(i = 0;i<M;i++)
            printf("%p-->%d\n",&arr1[i],arr1[i]);

        for(i = 0;i<M;i++)
         printf("%p-->%d\n",&arr2[i],arr2[i]);

        for(i = 0;i<M;i++)
         printf("%p-->%d\n",&arr3[i],arr3[i]);

         printf("%ld\t%ld\n",sizeof(arr0),sizeof(arr3));


        exit(0);

}

//输出
0x7fffa2d1c6e0-->1
0x7fffa2d1c6e4-->0
0x7fffa2d1c6e8-->-1383061459
0x7fffa2d1c6ec-->22061
    
0x7fffa2d1c6f0-->1
0x7fffa2d1c6f4-->2
0x7fffa2d1c6f8-->3
0x7fffa2d1c6fc-->4
    
0x7fffa2d1c700-->1
0x7fffa2d1c704-->3
0x7fffa2d1c708-->0
0x7fffa2d1c70c-->0
8	16

```



### 3. 元素引用

下标从０开始引用

```c
int arr[3]    //arr[o],arr[1],arr[3].
```

### 4. 数组名

> 数组的起始位置，定义后为一表示地址的常量

```c
int arr[3]
    printf('arr = %p\n',arr);	//输出arr[0]的存储位置
```

### 5. 数组越界

```ｃ
a[i] = *(a+i)	//以起始位置向上向下偏移取值
```

## 一维数组习题

### 1. 求fibonacci数列前十项，并从大到小存放

```c
# include <stdio.h>                                                              
# include <stdlib.h>

# define M 10
int main()
{

    int i,j,temp;
    int fib[M] = {1,1};
    printf("sizeof(fib) = %ld\tsizeof(fib[0])=%ld\n",sizeof(fib),sizeof(fib[0]));

    for(i=2;i<sizeof(fib)/sizeof(fib[0]);i++)
        fib[i] = fib[i-1] + fib[i-2];
    for(i=0;i<sizeof(fib)/sizeof(fib[0]);i++)
        printf("%d  ",fib[i]);
    printf("\n");

    	//beiqi君之不知什么种类的排序方法
    for(i=0;i<sizeof(fib)/sizeof(fib[0]);i++)
    {
            for(j=i;j<sizeof(fib)/sizeof(fib[0]);j++)
                    {
                        if(fib[i] < fib[j])
                        {
                            temp = fib[i];
                            fib[i] = fib[j];
                            fib[j] = temp;

                        }
                    }
    }			
    for(i=0;i<sizeof(fib)/sizeof(fib[0]);i++)
            printf("%d  ",fib[i]);
        printf("\n");
    exit(0);
}
```

### 2. 排序

> 冒泡，选择，快速

```c
//冒泡
# include <stdio.h>
# include <stdlib.h>

# define    M 10
int main()
{

        int i,j,k,temp;
        int sort[M]={1,55,3,23,54,78,32,41,22,66};

        for(i=0;i<M;i++)
            printf("%d  ",sort[i]);
        printf("\n");

        for(i=0;i<M-1;i++)
                for(j=0;j<M-i-1;j++)
                    if(sort[j] > sort[j+1])     
                    {
                            temp = sort[j+1];
                            sort[j+1] = sort[j];
                            sort[j] = temp;                                      
                    }
        for(i=0;i<M;i++)
            printf("%d  ",sort[i]);
        printf("\n");
        exit(0);
}

//快速
# include <stdio.h>
# include <stdlib.h>

# define    M 10
int main()
{

        int i,j,k,temp;
        int sort[M]={1,55,3,23,54,78,32,41,22,66};

        for(i=0;i<M;i++)
            printf("%d  ",sort[i]);
        printf("\n");
    
        for(i=0;i<M-1;i++)
            {
                t = i;
                for(j=i+1;j<M;j++)
                    if(sort[t] > sort[j])     
                    {
                            t = j;
                    }
                if(t != i)
                {
                        temp = sort[i];
                        sort[i] = sort[t];
                        sort[t] = temp;
                }
            }
        for(i=0;i<M;i++)
            printf("%d  ",sort[i]);
        printf("\n");
        exit(0);

｝
```

### 3. 进制转换

```c
# include <stdio.h>                                                             
# include <stdlib.h>


int main()
{

        static int n[128];
        int base,num;
        int i = 0;

        printf("please enter the converted num:");
        scanf("%d",&num);
        printf("please enter the base:");
        scanf("%d",&base);

        do
        {
            n[i] = num%base;
            num = num/base;
            i++;
        }while(num != 0);

        for(i--;i >=0;i--)
        {
                if(n[i] >= 10)
                        printf("%c",n[i]-10+'A');		//超过10转换为字符
                else
                        printf("%d",n[i]);
        }
        printf("\n");
        exit(0);
}
```

### 4. 删除法求质数

```c
// 从ａ开始删除后面能被ａ整除的数，之后ａ＋１

// 以下标当值实现

# include <stdio.h>
# include <stdlib.h>


int main()
{
        int i,j;
        char primer[1001] = {0};

        for(i = 2;i < 1001;i++)
        {
                if(primer[i] == 0)
                {
                        for(j = i*i; j < 1001; j+=i)                            
                                primer[j] = -1;
                }
        }
        
        for(i = 2;i<1001;i++)
                if(primer[i] == 0)
                    printf("%d ",i);
        printf("\n");
        exit(0);
}                                                                     
```

## 二维数组

### 1. 定义，初始化

> [存储类型] 数据类型 标识符[行下标] [列下标]

1. 不初始化－不确定
2. 全部初始化
3. 部分初始化－按顺序初始化，没初始化的部分为0
4. 行标可省略

### 2. 元素引用

> 数组名[行标] [列标]
>
> 数组名：数组的起始位置，定义后为一表示地址的常量

### 3. 存储形式

> 顺序存储，按行存储

```c
int arr[2][3] = {{2,3},{4}}
// arr00 = 2; arr01 = 3;arr02 = 0;arr10 = 4;arr11 = 0;arr12 = 0;
int arr[][3] = {2,3,4}
// arr00 = 2; arr01 = 3;arr02 = 4;arr10 = 0;arr11 = 0;arr12 = 0;
```

### 3. 深入理解二维数组

>行指针
>
>列指针

## 二维数组习题

```c
/*
1.行列互换
2.求最大值及其所在位置
3.求各行与各列的和
4.矩阵乘积
*/

#include <stdio.h>
#include <stdlib.h>

#define M 2
#define N 3
#define K 3

// 行列互换
static void change(void)
{


		int i,j;
		int a[M][N] = {1,2,3,4,5,6};
		int b[N][M];

		for(i=0;i<M;i++)
		{
				for(j=0;j<N;j++)
				{
						b[j][i] = a[i][j];
                        printf("%d ",a[i][j]);

				
				}
				printf("\n");
		}
		printf("\n");
		
		for(j=0;j<N;j++)
        {
                for(i=0;i<M;i++)
                {
                        printf("%d ",b[j][i]);

                }
                printf("\n");
        }


}
	
// 求最大值
static void max(void)
{

		int a[M][N] = {34,63,7,234,7,444};
		int i,j;
		int max = a[0][0];
		int row = 0,colum = 0;

		for(i=0;i<M;i++)
        {
                for(j=0;j<N;j++)
                {
                        printf("%d ",a[i][j]);

                }
                printf("\n");
        }

		for(i=0;i<M;i++)
        {
                for(j=0;j<N;j++)
						if(a[i][j] > max)
						{
								max = a[i][j];
								row = i+1;
								colum =j+1;
                		}
        }
		printf("max:%d,在第%d行%d列\n",max,row,colum);

}


// 求各行与各列的和
static void sum(void)
{
	// 设4行3列
		int i,j;
		int a[5][4] = {{2,55,3},{3,5,22},{43,6,2},{24,6,90}};
		for(i=0;i<4;i++)
				for(j=0;j<3;j++)
				{
						a[4][3] += a[i][j];
						a[i][3] += a[i][j];
						a[4][j] += a[i][j];
				}
	
        for(i=0;i<5;i++)
        {
                for(j=0;j<4;j++)
                {
                        printf("%4d ",a[i][j]);
                }
                printf("\n");
        }


}

// 矩阵相乘 前列=后行可相乘
static void mul(void)
{
        int a[M][N] = {{2,5,3},{3,5,2}};
        int b[N][K] = {{7,3,0},{9,1,2},{4,0,5}};
        int c[M][K] = {0};
		int i,j,k;


        for(i=0;i<M;i++)
        {
                for(j=0;j<N;j++)
                {
                        printf("%d ",a[i][j]);

                }
                printf("\n");
        }
		printf("\n");

		for(i=0;i<N;i++)
        {
                for(j=0;j<K;j++)
                {
                        printf("%d ",b[i][j]);
                }
                printf("\n");
        }

		printf("\n");
		for(i=0;i<M;i++)	//控制a的行
        {
                for(k=0;k<K;k++)	// 控制b的列
                {
                       	for(j = 0;j < N;j++)	// 控制a的列b的行
						c[i][k] += a[i][j] * b[j][k];
                }
        }

        for(i=0;i<M;i++)
        {
                for(j=0;j<K;j++)
                {
                        printf("%d ",c[i][j]);
                }
                printf("\n");
        }



}



int main()
{

		//	change();
		//	max();
		//	sum();
			mul();
		exit(0);
}

```



## 字符数组

### 1. 定义，初始化，存储特点

> [存储类型] 数据类型 标识符 [下标] . .

```c
//单个字符初始化
char str [10] = {'a','b','c'};	//abc	未定义的为'\0'

//字符串初始化
多一个'/0'.
系统对字符串常量也自动加一个'\0'作为结束符。例如"C Program”共有9个字符，但在内存中占10个字节，最后一个字节'\0'是系统自动加上的，一定会加。（通过sizeof()函数可验证
    
// 等价
char str[ ]="I am happy";
char str[ ]={'I',' ','a','m',' ','h','a','p','p','y','\0'};

```



### 2.输入输出

> 用scanf("%s");，无法获取带间隔的字符串，遇到间隔符自动结束本次scanf
>
> printf("");puts(str);

### 3. 常用函数

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define STRSIZE 10
/* strlen & sizeof			
 * strlen 以\0作为结束符，计算字符长度不含\0，sizeof计算字符真正占用的字节； 
 * 
 * strcpy & strncpy	//复制
 *      char *strcpy(char *dest, const char *src);
		char *strncpy(char *dest, const char *src, size_t n);限制复制数防止越界
 * 
 * strcat & strncat	//从\0处追加
 * 		char *strcat(char *dest, const char *src);
       	char *strncat(char *dest, const char *src, size_t n);限制追加数防止越界
 * 
 * strcmp & strncmp//ASCII比较，若str1=str2，则返回零；若str1<str2，则返回负数；若str1>str2，则返回正数
 * 		 int strcmp(const char *s1, const char *s2);
      	 int strncmp(const char *s1, const char *s2, size_t n);只比较前n位
 * */
int main()
{

		char str[STRSIZE] = "hello";
		char str1[STRSIZE] = "helloa";

		printf("%d\n",strcmp(str,str1));// -97
		printf("%d\n",strncmp(str,str1,5));// 0



/*
		char str[STRSIZE] = "hello";	//STRSIZE = 10
	//	strcat(str,"world!aaabbbccc");	//helloworld!aaabbbccc
		strncat(str,"world!aaabbbccc",STRSIZE)	//helloworld!aaab
		puts(str);

*/
		
/*
		char str[STRSIZE]; //STRSIZE = 5
		char str2[STRSIZE];

		strcpy(str,"sadsfafww"); //sadsfafww
		strncpy(str2,"asdsfsafww",STRSIZE);// sadsf

		puts(str);
		puts(str2);
*/

/*
		char str[] = "hello\0abc";
		printf("%d\n",strlen(str));		//5
		printf("%d\n",sizeof(str));		//10
*/
		exit(0);
}

```

### 字符数组习题

> 单词计数（以空格为分隔符）

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{

		char str[128];
		int i,count = 0,flag =0;

		gets(str);
		for(i=0;str[i] != '\0';i++)
		{
				if(str[i] == ' ')
						flag = 0;	//用0标记空格
				else //若为字符
					if(flag == 0)	//若上一个字符为空格
					{
							count ++;
							flag = 1;
					}
		}
		printf("单词数为：%d\n",count);
		exit(0);
}

```

## 多维数组
> 类似二维数组，指针
