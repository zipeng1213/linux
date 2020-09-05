## Shell

变量、条件、程序控制、命令、函数、内置命令

#### 脚本执行

1. bash 脚本文件
2. ./脚本文件
3. . 脚本文件（在当前shell中执行脚本（当前上下文中））

#### 变量

所有变量都是看作是字符串并以字符串的形式存储（赋值数值也是）

通过**$**来获取变量的值

加双引号""，不影响值的获取，"$myvar"

单引号会影响echo ‘$myvar’ 输出为$myvar

\相同				echo \\$myvar  输出为$myvar

局部变量

在代码块中使用关键字 local来声明局部变量，只能在代码块中使用

**将一个命令执行结果赋值给变量**

$()或者``

#### 环境变量

均为大写，会根据环境设置中的值类进行一个初始化（export用于在子shell中设置环境变量）

$0 shell脚本名

$1脚本参数

$# 脚本参数个数

$$脚本进程号

$@脚本所有参数会进行分割

$?上一个指令的执行状态返回值

#### 条件

test 或 [  ]

```shell
if [ -f file.c ]; then
...
fi
```

字符串比较

```
string1 = string2		是否相同(符号两边有空格)
string1 != string2
-n string		不为空结果为真
-z string 		为空结果为真
```

算数比较

```
a -eq b		相等
a -ne b		不相等
a -gt b		大于
a -ge b		大于等于
a -lt b		小于
a -le b		小于等于
```

文件测试条件

```
-d 		是否为目录
-f		是否为普通文件
-e		是否存在
-g      set-group-id是否设置
-u		set-user-id是否设置
-r
-w
-x
```

#### 控制结构

1. if

```
if [ -d file.c ]; then
...
elif [ -e file.c ]; then
...
fi
```

进行字符串判断时需要用双引号

2. for

```
for foo in $(ls ./); do
...
done
```

3. while

```
while [ "$str1" != "secret" ]; then
...
done
```

while 死循环

while . 		while true

4. case

```
case "$timeofday" in
[yY]|[Yy][Ee][Ss]) ehco "";;
no) ehco "";;
*) echo"";;
esac
```

#### 函数

```
func_name(){

}
```

$n表示函数的参数



#### 数组

定义

- array=(value1 value2 value3 ...)
- 采用键值对方式，array=([1]=one [2]=twe [3]=three)
- 通过分别定义数组方式，array[1]=one array[2]=twe 
- 动态定义数组 array=($(命令))

打印数组

${array[1]}获取单个元素

${array[*]}获取所有元素

${#array[*]}获取数组长度

数组截取${array[@]:1:3}获取1号到3号的元素

数组替换${array[@]/1/b}把数组中的1替换为b，格式${数组名[@或者*]/查找字符/替换字符}

#### 命令

外部命令+内部命令

1. break
2. ：

用于简化条件逻辑时，空命令相当于true的别名（while ：）

3. continue
4.  . 

用于在当前shell中执行命令，当shell中执行一条外部命令或者脚本程序时，会创建一个新的环境（子shell）通过source或者.命令会在当前shell上下文环境中执行，不会创建新的shell

5. echo

   -n去掉换行符	-e 确保启用了转义字符

6. eval

对参数进行求值

```
foo=10
a='$'foo
eval y=$a
echo $y
10
```

7. exec

将当前的shell替换为一个不同的程序，exec后面的代码都不会执行

8. exit

使脚本程序以退出码n结束运行；0为成功1~125是可以使用的错误代码，其余为保留

126文件不可执行	127命令未找到	128以上 出现一个信号

9. export

将他的参数导出到子shell中，并在子shell中有效；就是讲参数创建为一个环境变量这个环境变量可以被当前程序和调用的其他程序可见（可以使用命令 set；set -a 或者set -o allexport 导出后面所有的变量）

10. expr

参数当做一个表达式求值

```
a=1
b=2
c=$(expr $a + $b)    --->运算符两边有空格
expr $a \* $b		--->乘法要进行转义

判断是否为整数
i=5
expr $i+1 &>/dev/null
echo $?
0		-->表示为整数

计算字符串长度
char="i am boy"
expr length "$char"

```

算数运算

- let命令

```
let a=5*2
echo $a 		-->10
(只适用于整数，需要消耗一个变量)
```

- bc命令

```
echo "scale=3;8/3" | bc		--->2.666 scale设置精度，管道运算符
bc <<< "scale=3;8/3"		--->输入重定向
```

- $[运算表达式]

```
a=10
b=10
c=$[$a+$b]
```

- $(())

```
a=10
b=10
c=$(($a+$b))
```

- declare

```
声明变量为整数，直接进行整数运算
declare -i s
s=10/2			--->5
```

- awk

```
a=1
b=2
awk 'BEGIN{print '$a'+'$b'}'		--->3
```

11. printf

```
printf "%s %d\t%s" "a d" 12 as
```

12. return 

函数返回值，默认最后一条命令的退出码

13. set

为shell设置参数变量

14. shift

将参数左移一位

15. trap

在接受到信号后采取的行动 trap command signal

16. unset 

从环境中删除变量或者函数

#### 调用脚本模式

- fork

在父脚本中执行子脚本，会创建一个子shell，子shell可以从父shell中继承环境变量，子shell的环境变量不能带回父shell

- exec

在当前shell中执行

- source

在当前shell中执行，后面的代码继续执行



---

### Linux常用命令

