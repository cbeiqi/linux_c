[toc]



# 流程控制

> 顺序；
>
> 选择：出现一种以上情况；`if-else`	`switch-case`
>
> 循环：当某种条件成立情况下，重复执行某个动作 `while`	`do-while`	`for`	`if-goto`
>
> 辅助控制：`continue`	`break`

## if else

```c
// if	if-esle		if-esle if-else
//多个if
if (exp)
    if(exp)	//pi匹配esle
    	cmd；
esle 	//多个if与最近的一个if相匹配，可以只用if	可加括号避免出错
    cmd；
        
if (exp)
    cmd;
    esle if (exp)
        cmd;
        esle if(exp)
            cmd;
			esle 
                cmd;
```

## switch-case

```c
/*语法格式
	switch(op)			//灵活设置op
	{
		case 常量或常量表达式:
			break;				//不加break会继续执行下一个case
		case 常量或常量表达式:
			break;
		......
		default;
			sig；//结束并生成错误log
	}
*/
```

## whlie	 do-while

```c
/*语法格式：
    while:(最少执行次数为0)

    		while(exp)
                {
                	loop;
                }
	
	do-while:(最少执行次数为1)

			do
              {
                    loop;
                }while(exp);
*/

//	while计算1到100的和
#include <stdio.h>
#include <stdlib.h>

#define	LEFE	1
#define	RIGHT	100
int main()
{

		int i;	//循环变量
		int sum = 0;

		i = LEFE;
		while(i <= RIGHT)
				sum += i++;

		printf("sum = %d\n",sum);

		exit(0);

}

//	do-while计算1到100的和
#include <stdio.h>
#include <stdlib.h>

#define	LEFE	1
#define	RIGHT	100
int main()
{

        int i;  //循环变量
        int sum = 0;

        i = LEFE;

        do
        {
            sum += i++;
        }while(i <= RIGHT);

        printf("sum = %d\n",sum);
        exit(0);
}
```

## for	if-goto

```c
/*语法格式
	for(最少执行0次)
        for(exp1;exp2;exp3)	//分号不可省略	起始/终止/变化条件
        {
            loop;
        }
	if-goto(慎用，goto实现无条件跳转，且不能跨函数跳转，破坏函数结构
            if(exp)
            	goto loop;
*/

死循环：
    while(1);
	for(;;);
杀掉死循环：ctrl+c
    
```

## break	continue

```
	while()
	{
		......
		break;	//跳出本层while
		......
		continue;	// 跳出本次，执行下一次的while
	}
```

