## 一、Shell概述

Shell是一个命令行解释器，它接收应用程序/用户命令，然后调用操作系统内核。

Shell还是一个功能相当强大的编程语言，易编写，易调试，灵活性强。

## 二、Shell脚本入门

* 脚本格式：脚本以#！/bin/bash开头（指定解析器）

* 创建脚本，输出hello world

  ```shell
  # !/bin/bash
  echo "hello world"
  ```

* 脚本执行方式

  第一种：采用bash或sh+脚本的相对路径或绝对路径（不用赋予脚本执行权限）

  ```shell
  sh hello.txt
  # 相对路径
  ```

  第二种：采用输入脚本的绝对路径或相对路径执行脚本（需要赋予脚本执行权限）

  ```shell
  chmod +x scripts/hello,sh
  scripts/hello.sh
  ```

  如果执行的目录下面就有要执行的脚本，不能直接写脚本名字，需要输入 ./hello.sh

  第三种：在脚本的路径前加上”.“或者”source“

  ```shell
  source /root/scripts/hello.sh
  . /root/scripts/hello.sh
  ```

前两种方式都是在当前shell中打开一个子shell来执行脚本内容，当脚本内容结束，则子shell关闭，回到父shell中。

第三种可以使脚本内容在当前shell里执行，而无需打开子shell，这也是我们每次修改完/etc/profile文件以后，需要source一下的原因。

开子shell与不开子shell的区别在于环境变量的继承关系，如子shell中设置的当前变量，父shell是不可见的。

## 三、Shell变量

### 3.1系统预定义变量

常用系统变量：$HOME,$PWD,$SHELL,$USER等

查看系统变量的值：

```shell
[root@localhost scripts]# echo $HOME
/root
```

显示当前Shell中所有变量：set

```shell
set | less

ABRT_DEBUG_LOG=/dev/null
BASH=/bin/bash
BASHOPTS=checkwinsize:cmdhist:expand_aliases:extglob:extquote:force_fignore:histappend:interactive_comments:login_shell:progcomp:promptvars:sourcepath
BASH_ALIASES=()
BASH_ARGC=()
BASH_ARGV=()
BASH_CMDS=()
BASH_COMPLETION_
....
```

### 3.2自定义变量

1.基本语法

定义变量：`变量名=变量值`，=前后不能有空格

撤销变量：`unset 变量名`

声明静态变量：`readonly 变量`，不能unset

2.规则

* 变量名称可以由字母、数字和下划线组成，但不能以数字开头，环境变量建议大写
* 在bash中，变量默认类型为字符串类型，无法直接进行数值运算
* 变量的值如果有空格，需要使用双引号或单引号括起来
* 使用export导出，可以将局部变量变成全局变量

### 3.3特殊变量

#### 3.3.1 $n

n为数字，$0代表该脚本名称，$1-$9代表第一到第九个参数，十以上的参数需要用大括号包含，如${10}

```shell
#! /bin/bash

echo '=======================$n=========================='
echo script name:$0
echo 1st parameter:$1
echo 2st parameter:$2

```



```shell
[root@localhost scripts]# chmod +x parameter.sh
[root@localhost scripts]# ./parameter.sh abc def
=======================$n==========================
script name:./parameter.sh
1st parameter:abc
2st parameter:def
[root@localhost scripts]# 

```

#### 3.3.2 $ ############

获取所有输入参数的个数，常用于循环，判断参数的个数是否正确以及加强脚本的健壮性

```shell
echo '=======================$n=========================='
echo script name:$0
echo 1st parameter:$1
echo 2st parameter:$2
echo '=======================$#=========================='
echo parameter numbers: $#

parameter numbers: 2
```

#### 3.3.3 $@,$*

`$*`:代表命令行所有的参数，`$*`把所有参数看成一个整体

`$@`:也代表命令行所有的参数，`$@`把每个参数区别对待

```shell
#! /bin/bash

echo '=======================$n=========================='
echo script name:$0
echo 1st parameter:$1
echo 2st parameter:$2
echo '=======================$#=========================='
echo parameter numbers: $#
echo '=======================$*=========================='
echo $*
echo '=======================$@=========================='
echo $@

=======================$*==========================
abc def
=======================$@==========================
abc def

```

```shell
#!/bin/bash

echo '=====================$*==========================='
for para in "$*"
do
        echo $para
done


echo '=====================$@==========================='
for para in "$@"
do
        echo $para
done

[root@localhost scripts]# ./parameter_for_test.sh a b c d e
=====================$*===========================
a b c d e
=====================$@===========================
a
b
c
d
e
```



#### 3.3.4 $?

最后一次执行命令的返回状态，如果这个变量的值为0，证明上一个命令的正确执行；如果这个变量的值为非0（具体哪个数由命令自己决定），证明上个命令执行不正确

```shell
[root@localhost scripts]# echo $?
0
[root@localhost scripts]# parameter.sh
bash: parameter.sh: 未找到命令...
[root@localhost scripts]# echo $?
127
```

## 四、Shell运算符

`$((运算符))`或`$[运算符]`

双小括号里面可以用数学上的运算式

```shell
[root@localhost ~]# echo $[5*2]
10
[root@localhost ~]# a=$[6+8]
[root@localhost ~]# echo $a
14

[root@localhost ~]# a=$(expr 5 \* 2)
[root@localhost ~]# echo $a
10
[root@localhost ~]# a=`expr 5 \* 4`
[root@localhost ~]# echo $a
20
```

## 五、流程控制

### 5.1条件判断

#### 5.1.1基本语法：

（1）test condition

（2） [ condition ]  ==(condition前后要有空格)==

```shell
[root@localhost scripts]# a=hello
[root@localhost scripts]# test $a = Hello
[root@localhost scripts]# echo $?
1
[root@localhost scripts]# [ $a = Hello ]
[root@localhost scripts]# echo $?
1

```

注：判断书写时等号两边必须空格，否则测试一直结果为0，除非是[ ]情况下

```shell
[root@localhost scripts]# [ $a=Hello ]
[root@localhost scripts]# echo $?
0
[root@localhost scripts]# [ ]
[root@localhost scripts]# echo $?
1

```

#### 5.1.2 两个整数比较

-eq   等于  

-ne   不等于  

-lt  小于      

-le    小于等于          

-gt  大于    

-ge大于等于

#### 5.1.3 文件权限判断

-r 是否有读的权限

-w 是否有写的权限

-x  是否有执行的权限

```shell
[root@localhost scripts]# [ -r hello.sh ]
[root@localhost scripts]# echo $?
0
[root@localhost scripts]# [ -w hello.sh ]
[root@localhost scripts]# echo $?
0

```

#### 5.1.4 文件类型判断

-e 文件存在

-f 文件存在并是一个常规文件

-d 文件存在并且是一个目录

#### 5.1.5 多条件判断

&&表示前一条命令执行成功时，才执行后一条命令；||表示前一条命令执行失败后，才执行下一条命令

```shell
[root@localhost scripts]# [ $a -lt 20 ] && echo "$a<20" || echo "$a>=20"
15<20

```

### 5.2 if判断

1. 单分支

   ```shell
   if [ 条件判断式 ]; then
   	程序
   fi
   ```

2. 多分支

   ```shell
   if [ 条件判断式 ]
   then
   	程序
   elif [ 条件判断式 ]
   then
   	程序
   else
   	程序
   fi
   ```

   演示：

   ```shell
   [root@localhost scripts]# a=25
   [root@localhost scripts]# if [ $a -gt 18 ]; then echo OK;fi
   OK
   [root@localhost scripts]# a=15
   [root@localhost scripts]# if [ $a -gt 18 ]; then echo OK;fi
   ```



### 5.3 case语句

```shell
case $变量名 in
"值1")
	程序1
;;
"值2")
	程序2
;;
......
*)
	变量都不是以上的值，则执行此程序
;;
esac
```

注：

1. case行尾必须为单词”in“，每一个模式匹配必须以右括号”）“结束；
2. 双分号”;;“表示命令序列结束，相当于java中break；
3. 最后的”*）“表示默认模式，相当于java中default；

### 5.4 for循环

语法一：

```shell
for ((初始值；循环控制条件；变量变化))
do
	程序
done
```

语法二：

```shell
for 变量 in 值1 值2 值3..
do
	程序
done
```

```shell
[root@localhost scripts]# for i in {1..100}; do sum=$[$sum+$i]; done; echo $sum
5050

```

### 5.5 while循环

```shell
while [ 条件判断式 ]
do 
	程序
done
```

## 六、read读取控制台输入

`read （-p -t）(参数)`

-p:指定读取值时的提示符

-t:指定读取值时等待的时间，如果-t不加表示一直等待

参数：指定读取值的变量名

```shell
#!/bin/bash

read -t 10 -p "please tell me your name:" name
echo "welcome,$name"

[root@localhost scripts]# ./read_test.sh
please tell me your name:welcome,
[root@localhost scripts]# ./read_test.sh
please tell me your name:高鑫
welcome,高鑫
```

## 七、函数

### 7.1系统函数

#### 7.1.1 basename

`basename [string/pathname] [suffix]`

basename命令会删掉所有的前缀包括最后一个（'/')字符，然后将字符写出来

basename可以理解为取路径里的文件名称

suffix为后缀，如果suffix被指定了，basename会将pathname或string中的suffix去掉

```shell
[root@localhost scripts]# basename /home/scripts/hello.sh
hello.sh
[root@localhost scripts]# basename /home/scripts/hello.sh sh
hello.

```

#### 7.1.2 dirname

`dirname 文件绝对路径`

从给定的包含绝对路径的文件名中去除文件名（非目录部分），然后返回剩下的路径（目录的部分）

```shell
[root@localhost scripts]# dirname /root/scripts/parameter.sh
/root/scripts
[root@localhost scripts]# dirname ../scripts/parameter.sh
../scripts
[root@localhost scripts]# dirname ../sdjajts/parameter.sh
../sdjajts

```

### 7.2 自定义函数

```shell
[ function ]funname[()]
{
    Action;
    [return int;]
}
# []代表可省略
```

* 必须在调用函数之前，先声明函数，shell脚本是逐行运行，不会像其他语言一样先编译
* 函数返回值，只能通过$?系统变量获得，可以显示：return返回，如果不加，将以最后一条命令的运行结果，作为返回值。return后跟数值n（0-255）

```shell
#!/bin/bash

function  add(){
        s=$[ $1 + $2 ] 
        echo "result:" $s
} 
 
read -p "please input first integer:" a
read -p "please input second integer:" b

add $a $b

[root@localhost scripts]# vim fun_test.sh
[root@localhost scripts]# chmod +x fun_test.sh
[root@localhost scripts]# ./fun_test.sh
please input first intege:r443
please input second integer:333
result: 776
```

## 八、正则表达式

正则表达式使用单个字符串来描述、匹配一系列符合某个语法规则的字符串。在很多文本编辑器里，正则表达式通常用来检索、替换那些符合某个模式的文本。在Linux中，grep，sed，awk等文本处理工具都支持通过正则表达式进行模式匹配。

* 常规匹配：一串不包含特殊字符的正则表达式匹配它自己，例如：`grep gaoxin`，就会匹配所有包含gaoxin的行

* 特殊字符：

  ==^==匹配一行的开头，例如：

  ```shell
  [root@localhost ~]# cat /etc/passwd | grep ^a
  adm:x:3:4:adm:/var/adm:/sbin/nologin
  abrt:x:173:173::/etc/abrt:/sbin/nologin
  avahi:x:70:70:Avahi mDNS/DNS-SD Stack:/var/run/avahi-daemon:/sbin/nologin
  [root@localhost ~]# cat /etc/passwd | grep ^ad
  # 匹配所有以a开头的行
  ```

  ==$==匹配一行的结尾，例如：

  ```shell
  [root@localhost ~]# cat /etc/passwd | grep bash$
  root:x:0:0:root:/root:/bin/bash
  chen:x:1000:1000:cchen:/home/chen:/bin/bash
  ```

  ==.==匹配任意的字符,例如：

  ```shell
  [root@localhost ~]# cat /etc/passwd | grep r.t
  operator:x:11:0:operator:/root:/sbin/nologin
  sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
  # 会匹配包含"r_t"的所有行
  ```

  ==*==不单独使用，它和上一个字符连用，表示匹配上一个字符0次或多次

  ```shell
  [root@localhost ~]# cat /etc/passwd | grep ro*t
  root:x:0:0:root:/root:/bin/bash
  operator:x:11:0:operator:/root:/sbin/nologin
  abrt:x:173:173::/etc/abrt:/sbin/nologin
  rtkit:x:172:172:RealtimeKit:/proc:/sbin/nologin
  # 会匹配包含rt、rot、root...的字符
  ```

  ==[]==表示某个范围内的一个字符，例如：

  [6,8]---------匹配6或者8

  [0-9]------------匹配一个0-9的数字

  [0-9]*--------------匹配任意长度的数字字符串

  [a-z]---------------匹配一个a-z的之间的字符串

  [a-z]*----------------匹配任意长度的字母字符串

  [a-c,e-f]--------------匹配a-c或者e-f之间的任意字符

  ==\\==表示转义，并不会单独使用。由于所有单独特殊字符都有其特定匹配模式，当我们想匹配某一特殊字符本身时，就会碰到困难，此时需要将转义字符和特殊字符连用，来表示特殊字符本身。

## 九、文本处理工具

### 9.1 cut

cut的工作是在文件中负责剪切数据用的。cut命令从文件的每一行剪切字节、字符和字段并将这些字节、字段、字符输出。

`cut [-f -d -c] filename`

-f：列号，提取第几列

-d：分隔符，按照指定分隔分割列，默认是制表符"\t"

-c：按字符进行分割，后加n表示第几列-c 1

### 9.2 awk

把文件逐行的读入，以空格为默认分隔符将每行切片，切片的部分在进行分析处理

` awk [ -F -v] '/pattern1/{action1} /pattern{action2}...' filename`

pattern: 表示awk在数据中查找的内容，就是匹配模式

action:在找到匹配内容时执行的一系列命令

-F：指定输入文件分隔符

-v：赋值一个用户定义变量

```shell
# 搜索passwd文件以root关键字开头的所有行

[root@localhost scripts]# cat /etc/passwd |grep ^root | cut -d ":" -f 7
/bin/bash
[root@localhost scripts]# cat /etc/passwd | awk -F ":" '/^root/ {print $7}'
/bin/bash

```

awk内置变量：

FILENAME：文件名

NR：已读的记录号（行号）

NF：浏览记录的域的个数（切割后列的个数）

## 归档文件

```shell
#!/bin/bash

# 首先判段输入参数个数是否为1
if [ $# -ne 1 ]
then
        echo "参数个数错误！应该输入一个参数，作为归档目录名"
        exit
fi

# 从参数中获取目录名称
if [ -d $1 ]
then
        echo
else
        echo
        echo "目录不存在！"
        echo
        exit
fi

DIR_NAME=$(basename $1)
DIR_PATH=$(cd $(dirname $1); pwd)

# 获取当前日期
DATE=$(date+ %y%m%d)

# 定义生成的归档文件名称
FILE=archive_${DIR_NMAE}_$DATE.tar.gz
DEST=/root/archive/$FILE

# 开始归档
echo "start"
echo

tar -czf $DEST $DIR_PATH/$DIR_NAME

if [ $?o "result:" $s
-eq 0 ]
then
        echo
        echo "归档成功!"
        echo "归档文件为：$DEST"
        echo
else
        echo "归档出现问题！"
        echo
fi

exit

```



```shell
[root@localhost scripts]# mkdir /root/archive
[root@localhost scripts]# ./daily_archive.sh ../scripts
[root@localhost scripts]# crontab -e

0 2 * * * /root/scripts/daily_archive.sh /root/scripts
```

## 发送消息

```shell
#!/bin/bash

# 查看用户是否登录
login_user=$(who | grep -i -m  1 $1 | awk '{print $1}')

if [ -z $login_user ]
then
        echo "$1 不在线！"
        echo "脚本退出.."
        exit
fi

# 查看用户是否开启消息功能
is_allowed=$(who -T | grep -i -m  1 $1 | awk '{print $2}')

if [ $is_allowed != "+" ]
then
        echo "$1 没有开启消息功能"
        echo "脚本退出..."
        exit
fi

# 确认是否有消息发送
if [ -z $2 ]
then
        echo "没有消息发送"
        echo "脚本退出"
        exit
fi

# 从参数中获取要发送的信息
whole_msg=$(echo $* | cut -d " " -f 2-)

# 获取用户登录的终端
user_terminal=$(who | grep -i -m  1 $1 | awk '{print $2}')

# 写入要发送的消息
echo $whole_msg | write $login_user $user_terminal

if [ $? != 0 ]
then
        echo "发送失败!"
else
        echo "发送成功!"
fi

exit

```

```shell
[root@localhost scripts]# ./send_msg.sh root hi
发送成功!

Message from root@localhost.localdomain on pts/3 at 06:04 ...
hi
EOF

```








