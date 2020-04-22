---
layout: Bash
title: Bash
date: 2020-04-15 16:11:25
tags: Bash
categories:
- linux
---
## 运行Bash脚本
```Bash
# 使用shell来执行
$ sh hello.sh
# 使用bash来执行
$ bash hello.sh
#使用.来执行
$ . ./hello.sh
#使用source来执行
$ source hello.sh
#还可以赋予脚本所有者执行权限，允许该用户执行该脚本
$ chmod u+rx hello.sh
$  ./hello.sh
```
## 注释
```Bash
#!/bin/bash
echo "The # here does not begin a comment."
echo 'The # here does not begin a comment.'
echo The \# here does not begin a comment.
echo The # 这里开始一个注释
echo $(( 2#101011 ))     # 数制转换（使用二进制表示），不是一个注释，双括号表示对于数字的处理
```
## 分号 
- 使用分号（;）可以在同一行上写两个或两个以上的命令

```Bash
 #!/bin/bash
 echo hello; echo there
 filename=ttt.sh
 if [ -e "$filename" ]; then    # 注意: "if"和"then"需要分隔，-e用于判断文件是否存在
     echo "File $filename exists."; cp $filename $filename.bak
 else
     echo "File $filename not found."; touch $filename
 fi; echo "File test complete."
```

- 终止case选项(双分号)

```Bash
#!/bin/bash

varname=b

case "$varname" in
    [a-z]) echo "abc";;
    [0-9]) echo "123";;
esac
```

## 点号(.)
- 等价于source命令 在当前 bash 环境下读取并执行 FileName.sh 中的命令

```Bash
$ source test.sh
Hello World
$ . test.sh
Hello World
```

## 双引号(")
- 会阻止（解释）STRING中大部分特殊的字符

## 单引号(')
- 将会阻止STRING中所有特殊字符的解释,这是一种比使用”更强烈的形式

## 斜线(/)
- 文件名路径分隔符

## 反斜线(\\)
- 转移字符

## 反引号(`)
- 反引号中的命令会优先执行

```Bash
$ cp `mkdir back` test.sh back
```

## 冒号(:)
- 等价于"NOP" (no op) 也可以被认为与shell的内建命令true作用相同
- 与>重定向操作符结合会清空文件,不存在就创建
- 与>>不会对预先存在的目标文件产生影响,不存在就创建
- 也可能用来作为注释行
- 还用来在 /etc/passwd 和 $PATH 变量中做分隔符

```Bash
$ echo $PATH
/usr/local/bin:/bin:/usr/bin:/usr/X11R6/bin:/sbin:/usr/sbin:/usr/games
```

## 问号(?)
- 在一个双括号结构中，? 就是C语言的三元操作符

## 美元($)
- 变量替换

## 小括号(())
- 命令组 在括号中的命令列表，将会作为一个子 shell 来运行

```Bash
#!/bin/bash

a=123
( a=321; )

echo "$a" #a的值为123而不是321，因为括号将判断为局部变量
```

- 初始化数组

```Bash
#!/bin/bash

arr=(1 4 5 7 9 21)
echo ${arr[3]} # get a value of arr
```

## 大括号({})
- 文件名扩展 复制 t.txt 的内容到 t.back 中

```Bash
#!/bin/bash

if [ ! -w 't.txt' ];
then
    touch t.txt
fi
echo 'test text' >> t.txt
cp t.{txt,back}
```

- 代码块(内部组，这个结构事实上创建了一个匿名函数)然而，与“标准”函数不同的是,在其中声明的变量，对于脚本其他部分的代码来说还是可见的

```Bash
#!/bin/bash

a=123
{ a=321; }
echo "a = $a" # a=321
```

## 中括号([])
- 条件测试 (双中括号([[ ]])也用作条件测试(判断))

```Bash
#!/bin/bash

a=5
if [ $a -lt 10 ]
then
    echo "a: $a"
else
    echo 'a>=10'
fi
```

- 数组元素

```Bash
#!/bin/bash

arr=(12 22 32)
arr[0]=10
echo ${arr[0]} #10
```

## 尖括号(<>)
- test.sh > filename：重定向test.sh的输出到文件 filename 中。如果 filename 存在的话，那么将会被覆盖
- test.sh &> filename：重定向 test.sh 的 stdout（标准输出）和 stderr（标准错误）到 filename 中
- test.sh >&2：重定向 test.sh 的 stdout 到 stderr 中
- test.sh >> filename：把 test.sh 的输出追加到文件 filename 中。如果filename 不存在的话，将会被创建

## 竖线(|)
- 管道 分析前边命令的输出，并将输出作为后边命令的输入

## 破折号(-)
- 选项,前缀
- 用于重定向stdio或stdout

```Bash
#!/bin/bash
#下面脚本用于备份最后24小时当前目录下所有修改的文件.
BACKUPFILE=backup-$(date +%m-%d-%Y)
# 在备份文件中嵌入时间.
archive=${1:-$BACKUPFILE}
#  如果在命令行中没有指定备份文件的文件名,
#  那么将默认使用"backup-MM-DD-YYYY.tar.gz".

tar cvf - `find . -mtime -1 -type f -print` > $archive.tar
gzip $archive.tar
echo "Directory $PWD backed up in archive file \"$archive.tar.gz\"."

exit 0
```

## 波浪号(~)
- 表示home目录

## 变量
- 定义变量如: myname="test"
  - 变量名和等号之间不能有空格
  - 首个字符必须为字母（a-z，A-Z）
  - 中间不能有空格，可以使用下划线(_)
  - 不能使用标点符号
  - 不能使用bash里的关键字
- 使用变量
  - 变量名前加美元符号
  - 推荐给所有变量加花括号
  - 已定义的变量可以重新被定义
- 只读变量
  - 使用 readonly 命令可以将变量定义为只读变量
- 特殊变量
  - 局部变量 这种变量只有在代码块或者函数中才可见
  - 环境变量 这种变量将影响用户接口和 shell 的行为
- 位置参数
  - 从命令行传递到脚本的参数：0，1，2，3… 9之后的位置参数就必须用大括号括起来了，比如，{10}，{11}，11，{12}，0就是脚本文件自身的名字，1 是第一个参数
  - $# ： 传递到脚本的参数个数
  - $* ： 以一个单字符串显示所有向脚本传递的参数。与位置变量不同,此选项参数可超过 9
  - $$ ： 脚本运行的当前进程 ID号
  - $! ： 后台运行的最后一个进程的进程 ID号
  - $@ ： 与$*相同,但是使用时加引号,并在引号中返回每个参数
  - $： 显示shell使用的当前选项,与 set命令功能相同
  - $? ： 显示最后命令的退出状态。 0表示没有错误,其他任何值表明有错误
  
```Bash
#!/bin/bash

# 作为用例, 调用这个脚本至少需要10个参数, 比如:
# bash test.sh 1 2 3 4 5 6 7 8 9 10
MINPARAMS=10

echo

echo "The name of this script is \"$0\"."

echo "The name of this script is \"`basename $0`\"."


echo

if [ -n "$1" ]              # 测试变量被引用.
then
echo "Parameter #1 is $1"  # 需要引用才能够转义"#"
fi 

if [ -n "$2" ]
then
echo "Parameter #2 is $2"
fi 

if [ -n "${10}" ]  # 大于$9的参数必须用{}括起来.
then
echo "Parameter #10 is ${10}"
fi 

echo "-----------------------------------"
echo "All the command-line parameters are: "$*""

if [ $# -lt "$MINPARAMS" ]
then
 echo
 echo "This script needs at least $MINPARAMS command-line arguments!"
fi  

echo

exit 0
```

## 算数运算符
![](/img/bash/bash-1.png)
- 原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 bc,awk 和 expr，expr 最常用
- expr 是一款表达式计算工具，使用它能完成表达式的求值操作，使用反引号
- 表达式和运算符之间要有空格$a + $b写成$a+$b不行
- 条件表达式要放在方括号之间，并且要有空格[ $a == $b ]写成[$a==$b]不行
- 乘号（*）前边必须加反斜杠\才能实现乘法运算

```Bash
#!/bin/bash

a=10
b=20

val=`expr $a + $b`
echo "a + b : $val"

val=`expr $a - $b`
echo "a - b : $val"

val=`expr $a \* $b`
echo "a * b : $val"

val=`expr $b / $a`
echo "b / a : $val"

val=`expr $b % $a`
echo "b % a : $val"

if [ $a == $b ]
then
   echo "a == b"
fi
if [ $a != $b ]
then
   echo "a != b"
fi
```

## 关系运算符
![](/img/bash/bash-2.png)

```Bash
#!/bin/bash

a=10
b=20

if [ $a -eq $b ]
then
   echo "$a -eq $b : a == b"
else
   echo "$a -eq $b: a != b"
fi
```

## 逻辑运算符
![](/img/bash/bash-3.png)

```Bash
#!/bin/bash
a=10
b=20

if [[ $a -lt 100 && $b -gt 100 ]]
then
   echo "return true"
else
   echo "return false"
fi

if [[ $a -lt 100 || $b -gt 100 ]]
then
   echo "return true"
else
   echo "return false"
fi
```

## 字符串运算符
![](/img/bash/bash-4.png)

```Bash
#!/bin/bash

a="abc"
b="efg"

if [ $a = $b ]
then
   echo "$a = $b : a == b"
else
   echo "$a = $b: a != b"
fi
if [ -n $a ]
then
   echo "-n $a : The string length is not 0"
else
   echo "-n $a : The string length is  0"
fi
if [ $a ]
then
   echo "$a : The string is not empty"
else
   echo "$a : The string is empty"
fi
```

## 文件测试运算符
![](/img/bash/bash-5.png)

```Bash
#!/bin/bash

file="/home/shiyanlou/test.sh"
if [ -r $file ]
then
   echo "The file is readable"
else
   echo "The file is not readable"
fi
if [ -e $file ]
then
   echo "File exists"
else
   echo "File not exists"
fi
```

## 流程控制
- if

```Bash
if condition
then
    command1 
    command2
    ...
    commandN 
fi
```

- if else

```Bash
if condition
then
    command1 
    command2
    ...
    commandN
else
    command
fi
```

- if-elif-else

```Bash
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

- for

```Bash
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done

for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done

for str in This is a string
do
    echo $str
done
#This
#is
#a
#string
```

- while

```Bash
while condition
do
    command
done

#!/bin/bash
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done

#输入信息被设置为变量MAN，按结束循环
echo 'press <CTRL-D> exit'
echo -n 'Who do you think is the most handsome: '
while read MAN
do
    echo "Yes! $MAN is readlly handsome"
done

#死循环
while :
do
    command
done

while true
do
    command
done

for (( ; ; ))
```

- until 

```Bash
#直至条件为真时停止
until condition
do
    command
done
```

- case
  - 取值后面必须为单词in，每一模式必须以右括号结束。取值可以为变量或常数。匹配发现取值符合某一模式后，其间所有命令开始执行直至 ;;
  - 取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令
  
```Bash
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2）
    command1
    command2
    ...
    commandN
    ;;
esac

echo 'Enter a number between 1 and 4:'
echo 'The number you entered is:'
read aNum
case $aNum in
    1)  echo 'You have chosen 1'
    ;;
    2)  echo 'You have chosen 2'
    ;;
    3)  echo 'You have chosen 3'
    ;;
    4)  echo 'You have chosen 4'
    ;;
    *)  echo 'You did not enter a number between 1 and 4'
    ;;
esac

#Enter a number between 1 and 4:
#The number you entered is:
#3
#You have chosen 3
```

- break

```Bash
#!/bin/bash
while :
do
    echo -n "Enter a number between 1 and 5:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "The number you entered is $aNum!"
        ;;
        *) echo "The number you entered is not between 1 and 5! game over!"
            break
        ;;
    esac
done
#Enter a number between 1 and 5:3
#The number you entered is 3!
#Enter a number between 1 and 5:7
#The number you entered is not between 1 and 5! game over!
```

- continue

```Bash
#!/bin/bash
while :
do
    echo -n "Enter a number between 1 and 5: "
    read aNum
    case $aNum in
        1|2|3|4|5) echo "The number you entered is $aNum!"
        ;;
        *) echo "The number you entered is not between 1 and 5!"
            continue
            echo "game over"
        ;;
    esac
done
```

## 函数定义
- 可以带function fun() 定义，也可以直接fun() 定义,不带任何参数
- 参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255)
- 函数返回值在调用该函数后通过 $? 来获得
- 所有函数在使用前必须定义

```Bash
[ function ] funname [()]

{

    action;

    [return int;]

}

#!/bin/bash
funWithReturn(){
    echo "This function will add the two numbers of the input..."
    echo "Enter the first number: "
    read aNum
    echo "Enter the second number: "
    read anotherNum
    echo "The two numbers are $aNum and $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "The sum of the two numbers entered is $? !"
```

## 函数参数

```Bash
#!/bin/bash
funWithParam(){
    echo "The first parameter is $1 !"
    echo "The second parameter is $2 !"
    echo "The tenth parameter is $10 !"
    echo "The tenth parameter is ${10} !"
    echo "The eleventh parameter is ${11} !"
    echo "The total number of parameters is $# !"
    echo "Outputs all parameters as a string $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73

#The first parameter is 1 !
#The second parameter is 2 !
#The tenth parameter is 10 !
#The tenth parameter is 34 !
#The eleventh parameter is 73 !
#The total number of parameters is 11 !
#Outputs all parameters as a string 1 2 3 4 5 6 7 8 9 34 73 !
#$10不能获取第十个参数
```


