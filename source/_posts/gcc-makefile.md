---
layout: gcc
title: gcc&makefile
date: 2020-04-09 10:25:58
tags: gcc
categories:
- linux
---
# gcc 与 makefile

## gcc

  - -E  预编译
  - -S  汇编
  - -c  编译
  - -o 链接
  - -g  产生调试信息
  - -D  宏定义
  - -I  include文件位置
  - -L 连接库位置
  - -l  库名
  - -O 优化选项 -O1，-O2，-O3
  - -Wall  显示警告

## makefile

  - 显示规则

```makefile
      target：dependency
      	cd
```

  - 变量

```makefile
      #定义变量
      OBJ=
      OBJ:=
      OBJ+=
      #引用变量
      $(OBJ)
```

  - 通配符

    - %任意一个
    - *所有
    - ？匹配一个

  - 常量

    - $@:代表目标文件$(TARGET)
    - $^:代表依赖文件
    - $<:代表第一个依赖文件

  - 隐示规则：make有智能性，能推断生成出%.o依赖%.c

## 程序发布流程

  - 查看elf文件
    - readelf 命令
  - 生成符号表
    - objcopy --only-keep-debug test test.symbol
  - 去掉符号表(release)
    - objcopy --strip-debug test test.release
    - strip test.release
  - 添加符号表(debug)
    - gdb -q -symbol=test.symbol --exec=test.release
  - 查看符号表
    - symbol-file ./test.symbol

## 单目录makefile模板

```makefile
SRC=$(wildcard *.c) 			#获取目录所有.c文件
OBJ=$(patsubst %.c,%.o,$(SRC)) 	#替换.c为.o
TARGET= 						#目标
INCLUDE=						#头文件
DEFS=							#宏定义
CFLAGES= -g						#参数
CC= gcc							#编译器
LIB_PATH= -L/home/lib/			#第三方库位置
LIBS= -lpthread					#库名称
RELEASE= release			
SYMBOL= symbol

$(TARGET):$(OBJ)
	$(CC) $(CFLAGES) -o $@ $^ $(LIB——PATH) $(LIBS)

$(RELEASE):$(TARGET)
	objcopy --strip-debug $(TARGET) $(TARGET).$(RELEASE)
	strip $(TARGET).$(RELEASE)
	
$(SYMBOL):$(TARGET)
	objcopy --only-keep-debug $(TARGET) $(TARGET).$(SYMBOL)
	
.PHONY:
clean:
	rm -rf *.o $(TARGET) $(TARGET).$(RELEASE) $(TARGET).$(SYMBOL) 
```