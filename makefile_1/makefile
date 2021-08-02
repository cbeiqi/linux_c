#注释
OBJS=main.o tool1.o tool2.o
CC=gcc
CFLAGS+=-c -Wall -g

mytool:$(OBJS)
	$(CC) $^ -o $@

%.o:%.c
	$(CC) $^ $(CFLAGS) -o $@

clean:
	rm -rf *.o mytool



