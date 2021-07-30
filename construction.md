[toc]



# 构造类型

## struct 结构体

### 1. 介绍

> 允许内部的元素是不同类型的

### ２. 类型描述

```c
struct 结构体名
{
   		数据类型	成员１;
    	数据类型	成员２;
    	......
};
//eg
struct simple_st                                   
{
        int i,j;
        float f;
        char ch;
    	struct aa bb; 
};

struct                            
{
        int i,j;
        float f;
        char ch;
    	struct aa bb; 
}a;		//无名结构体
```

### 3. 嵌套定义

> struct 中成员有struct类型/union类型

### 4. 定义变量，初始化，成员引用

> 成员引用方法：
>
> 1. 变量名.成员名
>
> 2. 指针-> 成员名
>
>    （*指针).成员名

```c
# include<stdio.h>                                                              
# include<stdlib.h>

#define NAMESIZE 32
// 结构体定义一般放于函数外

struct birth_st
{
        int year;
        int month;
        int day;
};

struct student_st       //描述一个学生的信息
{
        char name[NAMESIZE];
        int id;
        int math;
        int Chinese;
        struct birth_st birth;  ////嵌套定义
/*      struct birth_st
{
        int year;
        int month;
		int day;
}birth;
*/

};


int main()
{
        // TYPE NAME = VALUE;
        
    //  struct student_st stu = {.name = "Beiqi",.math = 100};
    //  printf("%s %d\n",stu.name,stu.math);    //部分赋值
    
        int i;
        struct student_st stu = {"Beiqi",31180,100,99,{1999,03,26}};  //全部赋值
        struct student_st *p = &stu;
        struct student_st stuarr[2] = {{"Beiqi",31180,100,99,{1999,03,26}},{"Mike",31181,50,50,{1999,3,22}}};

        printf("%s %d %d %d %d-%d-%d\n",stu.name,stu.id,stu.math,stu.Chinese,stu.birth.year,stu.birth.month,stu.birth.day);
        
        //通过指针引用
        printf("%s %d %d %d %d-%d-%d\n",p->name,p->id,p->math,p->Chinese,p->birth.year,p->birth.month,p->birth.day);
        //(*p)相当于一个变量名
        printf("%s %d-%d-%d\n",(*p).name,(*p).birth.year,(*p).birth.month,(*p).birth.day);
        
        p = stuarr;
        for(i = 0;i < 2;i++,p++)
        printf("%s %d %d %d %d-%d-%d\n",p->name,p->id,p->math,p->Chinese,p->birth.year,p->birth.month,p->birth.day);
    
        exit(0);
}                                                                               
/*
Beiqi 31180 100 99 1999-3-26
Beiqi 31180 100 99 1999-3-26
Beiqi 1999-3-26
Beiqi 31180 100 99 1999-3-26
Mike 31181 50 50 1999-3-22
*/
```

### 5. 占用内存空间大小

对齐规则 :[CSDN结构体内存对齐规则](https://blog.csdn.net/weixin_45157820/article/details/112755832)

```c
#include<stdio.h>
#include<stdlib.h>

struct eg
 {
    char    a;
    short   b;
    char    ff;
    int     c;
 };

int main()
{
        struct eg d;
        printf("%p\n%p\n%p\n%p\n",&d.a,&d.b,&d.ff,&d.c);                    
        printf("%d",sizeof(d));
        exit(0);
}
/*
0x7ffd57f9922c
0x7ffd57f9922e
0x7ffd57f99230
0x7ffd57f99234
12
    
*0**
*000
****
```

> 对齐对网络传输有影响，指定不对其齐

```c
# include<stdio.h>
# include<stdlib.h>

// #pragma pack(1)      //指定对齐值
struct simple_st
{                                                                               
        int i;
        char ch;
        float f;
}__attribute__((packed));   //指定当前不要对齐

int main()
{

        struct simple_st a;
        struct simple_st *p = &a;

        printf("sizeof(p) = %d\n",sizeof(p));
        printf("sizeof(struct simple_st) = %d\n",sizeof(a));
        printf("%p\n%p\n%p\n",&a.i,&a.ch,&a.f);                                 
        exit(0);
}
/*
sizeof(p) = 8
sizeof(struct simple_st) = 9
0x7fff5df9a92f
0x7fff5df9a933
0x7fff5df9a934

```

###  6. 函数传参

```c
# include<stdio.h>
# include<stdlib.h>

struct simple_st
{                                                                               
        int i;
        char ch;
        float f;
};

void func1(struct simple_st tmp)
{
        printf("%d\n",sizeof(tmp));   //12                                            
}

void func2(struct simple_st *p)
{
        printf("%d\n",sizeof(p));   //8                                         
}
int main()
{

        struct simple_st a;
        struct simple_st *p = &a;

        func1(a); //相当于传了a.i,a.ch,a.f，对形参开销大，一般不用
        
        func2(p);   //相当于func(&a)
        exit(0);
}
```

> 1.当一种struct类型作为参数传递后，那么他同一个内置类型传递是一样的！一个int整形传递时，拷贝了4个字节(32位系统固定大小)！同样，struct类型也要拷贝他的固定大小！
> PS：所以很多时候，为了速度上的需要，对于构造类型，都建议传递指针(32位4个字节)，因为这样传递要减少很多的拷贝！
>
> 2.对于同种的类型的构造struct类型，同内置类型一样！可以赋值！两个int整形可以赋值，同样同种struct类型也一样！这里的赋值也是对总内存大小进行值拷贝

## Union共用体

### 1. 介绍

> 结构体和共用体的区别在于：结构体的各个成员会占用不同的内存，互相之间没有影响；而共用体的所有成员占用同一段内存，修改一个成员会影响其余所有成员。
>
> 结构体占用的内存大于等于所有成员占用的内存的总和（成员之间可能会存在缝隙），共用体占用的内存等于最长的成员占用的内存。共用体使用了内存覆盖技术，同一时刻只能保存一个成员的值，如果对新的成员赋值，就会把原来成员的值覆盖掉。

### 2.类型描述

```c
union 结构体名
{
   		数据类型	成员１;
    	数据类型	成员２;
    	......
};
//eg
union simple_un                                   
{
        int i,j;
        float f;
        char ch;
};

union                
{
        int i,j;
        float f;
        char ch;
}a;		//无名共用体体
```

### 3.嵌套定义

> union 中成员有struct类型/union类型

```c
# include<stdio.h>                                                              
# include<stdlib.h>
# include<stdint.h>
//嵌套定义
//计算32位数高十六位与低十六位的和

union
{
        struct
        {
                uint16_t i;
                uint16_t j;
        }a;
        uint32_t k;

}b;

int main()
{

        /*
        int num1 = 0x111223344;
        printf("%#x\n",(num1 >> 16) + (num1 & 0xFFFF)); //4466
        */
        b.k = 0x11223344;       //定义k
        printf("%#x\n",b.a.i + b.a.j);  //间接使用a.i,a.j输出
        exit(0);
}
//0x4466
```



### 4.定义变量，初始化，成员引用

```c
#include<stdio.h>
#include<stdlib.h>

union simple_un                                  
{
        int i,j;
        float f;
        double d;
        char ch;
};

int main()
{

        union simple_un a;
        union simple_un *p = &a;
        union simple_un arr[3];
        a.i = 2;
        a.f = 123.55;

        printf("%ld\n",sizeof(a));
                                                                                
        // 多个成员占用一个块空间，同一时间只有一个值生效
        printf("%d %f\n",a.i,p->f);//1123490202 123.550003只有f有效
        
        exit(0);
}

```

### 5. 占用内存大小

> 按最大成员的大小分配空间

### 6.函数传参

> 类似结构体，传地址更省形参开销

### 7.位域

> 有些数据在存储时并不需要占用一个完整的字节，只需要占用一个或几个二进制位即可。例如开关只有通电和断电两种状态，用 0 和 1 表示足以，也就是用一个二进位。
>
> 我们可以这样认为，位域技术就是在成员变量所占用的内存中选出一部分位宽来存储数据。

## enum 枚举

### 1. 介绍

> 有点像#define
>
> 能够列出所有可能的取值，并给它们取一个名字

### 2. 类型描述

```c
//列出一个星期有７天
enum week{ Mon = 1, Tues = 2, Wed = 3, Thurs = 4, Fri = 5, Sat = 6, Sun = 7 };//不赋值从前一个开始递增，即０－６
enum week{ Mon = 1, Tues =, Wed =, Thurs =１ Fri, Sat, Sun  }；
    // 1,2,3,1,2,3,4
```

> 可应用switch case语句中