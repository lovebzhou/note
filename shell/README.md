## 1. 概述

进入终端编写hello.sh：
```Shell
#!/bin/bash

echo 'hello world!'
```
运行：
```shell
# 方法1 
sh hello.sh  

# 方法2 
chmod u+x hello.sh 
./hello.sh
```
-   `#!` 告诉系统这个脚本需要什么解释器来执行。
-   文件扩展名 `.sh` 不是强制要求的。
-   方法1 直接运行解释器，`hello.sh` 作为 Shell 解释器的参数。此时 Shell 脚本就不需要指定解释器信息，第一行可以去掉。
-   方法2 hello.sh 作为可执行程序运行，Shell 脚本第一行一定要指定解释器。

### 注释

以 # 开头的行就是注释，会被解释器忽略。
```sh
# =>>> 以 # 开头的行就是注释，会被解释器忽略。
# 这是一个注释  

# =>>> 多行注释还可以使用以下格式：
# :<<EOF  
# 注释内容...
# 注释内容...
# EOF

# EOF 也可以使用其他符号:
:<<'  
注释内容...
注释内容...
'

:<<!
注释内容...
注释内容...
!
```

## 2. 变量

Shell 变量分为**系统变量**和**自定义变量**。系统变量有`$HOME`、`$PWD`、`$USER`等。

显示当前 Shell 中所有变量：`set` 。  

变量名可以由字母、数字、下划线组成，不能以数字开头。  

-   **定义变量**：变量名=变量值，等号两侧不能有空格，变量名一般习惯用大写。
-   **删除变量**：unset 变量名 。
-   **声明静态变量**：readonly 变量名，静态变量不能unset。
-   **使用变量**：$变量名

**将命令返回值赋给变量（重点）**

-   A=\`ls\` 反引号,执行里面的命令
-   A=$(ls) 等价于反引号

### 环境变量

`~/.bash_profile`, `~/.zshrc`

1.  export 变量名=变量值，将 Shell 变量输出为环境变量。
2.  source 配置文件路径，让修改后的配置信息立即生效。
3.  echo $变量名，检查环境变量是否生效

### 位置参数

```bash
#!/bin/bash     

# $n ：`$0` 代表命令本身、$1-$9 代表第1到9个参数，10以上参数用花括号，如 ${10}。
echo $0 $1 $2 

# $* ：命令行中所有参数，且把所有参数看成一个整体。
echo $* 

# $@：命令行中所有参数，且把每个参数区分对待。
echo $@ 

# $#：所有参数个数。
echo 参数个数=$#
```

### 预定义变量  

```bash
#!/bin/bash

# $$：当前进程的 PID 进程号。
echo 当前的进程号=$$ 

# $!：后台运行的最后一个进程的 PID 进程号。
./hello.sh & # &：以后台的方式运行程序 
echo 最后一个进程的进程号=$! 

# $?：最后一次执行的命令的返回状态，0为执行正确，非0执行失败。
echo 最后执行的命令结果=$?
```

### 字符串

```sh

# =>>> 单引号
# - 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
# - 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。
str='this is a string'

# =>>> 双引号
# - 双引号里可以有变量
# - 双引号里可以出现转义字符
your_name="runoob"  
str="Hello, I know you are \"$your_name\"! \n"  
echo -e $str # Hello, I know you are "runoob"!


your_name="runoob"  
# 使用双引号拼接  
greeting="hello, "$your_name" !"  
greeting_1="hello, ${your_name} !"  
echo $greeting  $greeting_1  
  
# 使用单引号拼接  
greeting_2='hello, '$your_name' !'  
greeting_3='hello, ${your_name} !'  
echo $greeting_2  $greeting_3
```
### 数组

```sh
# =>>> 定义

# 数组名=(值1 值2 ... 值n)
arr=(value0 value1 value2 value3)

# 或
arr=(
value0
value1
value2
value3
)

# 还可以单独定义数组的各个分量：
arr[0]=value0
arr[1]=value1
arr[n]=valuen

# =>>> 读取数组元素值的一般格式是：${数组名[下标]}
valuen=${arr[n]}

# 使用 @ 符号可以获取数组中的所有元素
echo ${arr[@]}

# 取得数组元素的个数  
length=${#arr[@]}  
# 或者  
length=${#arr[*]}  
# 取得数组单个元素的长度  
lengthn=${#arr[n]}

# =>>> 遍历
for i in "${arr[@]}"; do
	echo "$i"
done

# =>>> 数值中是否包含value
if [[ "${arr[*]}" =~ "value0" ]]; then
	echo "1111"
else
	echo "2222"
fi
```
## 3. 运算符

-   $((运算式)) 或 $[运算式]
-   expr m + n 注意 expr 运算符间要有空格
-   expr m - n
-   expr \*，/，% 分别代表乘，除，取余
```bash
# 第1种方式 $(()) 
echo $(((2+3)*4))   

# 第2种方式 $[]，推荐 
echo $[(2+3)*4]  

# 使用 expr 
TEMP=`expr 2 + 3` 
echo `expr $TEMP \* 4`
```

## 4. 控制流

### if
```bash
#!/bin/bash
if [ $1 -ge 60 ]
then
    echo 及格
elif [ $1 -lt 60 ]
then
    echo "不及格" 
fi
```
### case
```bash
case $1 in
"1")
echo 周一
;;
"2")
echo 周二
;;
*)
echo 其它
;;
esac
```

### for
```bash
#!/bin/bash  

echo "=>>>使用\$\*:" 
for i in "$*" 
do     
    echo "the arg is $i" 
done 

echo "=================="  

echo "=>>>使用\$\@:"
for j in "$@" 
do     
    echo "the arg is $j" 
done


SUM=0  
for ((i=1;i<=100;i++)) 
do     
    SUM=$[$SUM+$i] 
done 

echo $SUM
```

### while
```bash
#!/bin/bash

SUM=0
i=0

while [ $i -le $1 ]
do
    SUM=$[$SUM+$i]
    i=$[$i+1]
done       
echo $SUM
```

## 5. 输入&输出

```bash
#!/bin/bash

# -p: 指定读取值时的提示符
read -p "请输入一个数num1=" NUM1
echo "你输入num1的值是：$NUM1"

# -t: 指定读取值时等待的时间（秒），如果没有在指定时间内输入，就不再等待了。
WAIT=5
read -t $WAIT -p "请在${WAIT}秒内输入一个数num2=" NUM2
echo ""
echo "你输入num2的值是：$NUM2"
```

## 6. 函数

### 系统函数

```bash
#
# basename: 删掉路径最后一个 / 前的所有部分（包括/），常用于获取文件名。
#

# sort
echo `basename /usr/bin/sort`

# stdio.h"
echo `basename include/stdio.h`

#
# dirname: 删掉路径最后一个 / 后的所有部分（包括/），常用于获取文件路径。
#
echo `dirname /a/b/c/d.txt`
```

### 自定义函数
```bash
#!/bin/bash

function getSum(){
    SUM=$[$n1+$n2]
    echo "sum=$SUM"
}   

read -p "请输入第一个参数n1：" n1
read -p "请输入第二个参数n2：" n2

# 调用 getSum 函数
getSum $n1 $n2
```


## 参考资料
[掌握Shell编程，一篇就够了](https://zhuanlan.zhihu.com/p/102176365)