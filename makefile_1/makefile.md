# makefile工程文件的编写规则

```c
targets : prerequisites
    command
目标：依赖
    命令
```

```makefile
#基本格式
mytool:main.o tool1.o tool2.o
    gcc main.o tool1.o tool2.o -o mytool

main.o:main.c
    gcc main.c -c -Wall -g -o main.o
too1.o:tool1.c
    gcc tool1.c -c -Wall -g -o tool1.o
tool2.o:tool2.c
    gcc tool2.c -c -Wall -g -o tool2.o


# make clean = rm -rf *.o mytool
clean:
    rm -rf *.o mytool

```

> 使用变量名 $为取值符号

```makefile
OBJS=main.o tool1.o tool2.o                                                     
CC=gcc
CFLAGS+=-c -Wall -g

mytool:$(OBJS)
    $(CC) $(OBJS) -o mytool

main.o:main.c
    $(CC) main.c $(CFLAGS) -o main.o
too1.o:tool1.c
    $(CC) tool2.c $(CFLAGS) -o tool2.o
tool2.o:tool2.c
    $(CC) tool2.c $(CFLAGS) -o tool2.o


clean:
    rm -rf *.o mytool
```

> `%`为通配符  	`$^`表示上一次依赖 		`$@`表示上一次目标

```makefile
OBJS=main.o tool1.o tool2.o
CC=gcc
CFLAGS+=-c -Wall -g

mytool:$(OBJS)
    $(CC) $^ -o $@

%.o:%.c
    $(CC) $^ $(CFLAGS) -o $@

clean:
    rm -rf *.o mytool
```

