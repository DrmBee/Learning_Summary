# shell学习

#### 1. shell 简介

shell是一个用C语言编写的程序，它是用户使用linux的桥梁。shell既是一种命令语言，又是一种程序设计语言。

shell是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

`第一个shell脚本`

```
#!/bin/bash
echo "Hello world!"
```

`#!`是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种shell。

`运行shell脚本有两种方法：`

- 作为可执行程序

​		将上面的代码保存为test.sh，并cd到相应目录：

```
chmod +x ./test.sh	# 使脚本具有执行权限
./test.sh	# 执行权限
```

注意：需要写成`./test.sh`，不能写成`test.sh`，直接写成test.sh，linux系统会去PATH里寻找有没有叫 test.sh的，显然不正确！！！通常只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里。

- 作为解释器参数

直接运行解释器，其参数就是shell脚本的文件名。例如：

```
/bin/sh test.sh
/bin/php test.php
```

这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没有用。

#### 2. shell 变量

- 定义变量时，变量名不加美元符号（$，PHP语言中变量需要），如：

```
your_name='example'
```

注意：变量名和等号之间不能有空格。

除了显式地直接赋值，还可以用语句给变量赋值，如：

```
for file in `ls /etc`
或者
for file in $(ls /etc)
# 以上语句将 /etc 下目录的文件名循环出来。
# $() 与 反引号 `` 其实效果是一样的
# 作用：执行命令并返回结果。不对内部命令的特殊字符进行转译。
# 将shell命令引起来，可以将命令的输出值赋给变量。
```

- 使用变量

使用一个定义过的变量，只要在变量名前加美元符号即可，如：

```
your_name="qinjx"
echo $your_name
echo ${your_name}
```

注意：使用变量的时候才加`$`，定义变量的时候不加`$`。

例子：

```
for skill in Ada Coffe Action Java; do
    echo "I am good at ${skill}Script"
done
for file in $(ls ./); do
	echo ${file}
done
echo "Hello world!"
sleep 10000
```

注意：

1. 对于for循环内使用其变量的时候要用` ${}`获取变量值，使用`for var in ...; do 命令 done`的格式。
2. 注意`$()`和`${}`的区别。
3. `sleep 10000`命令可以让bash窗口暂留。



- 只读变量

使用`readonly命令`可以将变量定义为只读变量，只读变量的值不能被改变。

```
#!/bin/bash
myurl="https://github.com/jhu99/miteFinder/tree/master"
readonly myurl
```

- 删除变量

使用`unset命令`可以删除变量。语法：

```
unset variable_name
```

变量被删除后不能再次使用。unset命令不能删除只读变量。

- 变量类型

运行shell时，会同时存在三种变量：

1. 局部变量 局部变量在脚本或者命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
2. 环境变量 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
3. shell变量 shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行。

------

- shell 字符串

字符串是shell编程中最常用最有用的数据类型，字符串可以用单引号，也可以用双引号，也可以不用引号。

1. 单引号

```
str='this is a string'
```

单引号字符串的限制：

​		a. 单引号里的任何字符串都会原样输出，单引号字符串中的变量是无效的；

​		b. 单引号字符串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

2. 双引号

```
your_name='liuying'
str="hell0,my name is \"${your_name}\"!\n"
echo -e ${str}	# -e选项表示启用转义字符的解释。
```

双引号的优点：

​		a. 双引号里可以有变量；

​		b. 双引号里可以出现转义字符；

- 拼接字符串

```
your_name="liuying"
# 使用双引号拼接
greeting="hello,"$your_name"!"
greeting_1="hello,${your_name}!"
greeting_2="hello,$your_name!"
echo $greeting $greeting_1 $greeting_2
# $后面的括号可省
# 双引号中的双引号可省

# 使用单引号拼接
greeting_3='hello,'$your_name' !'
greeting_4='hello,${your_name} !'
echo $greeting_3 $greeting_4
```

注意：在引号内部，无论是单引号还是双引号都是拼接符号！！！和最外面的引号意义不同！！！

- 获取字符串长度

```
string="abcd"
echo ${#string}	# 输出4
```

变为数组时，`${#string}`等价于`${#string[0]}`:

```
string="abcd"
echo ${#string[0]}	#输出4
```

`${#变量名}` 表示变量的长度

- 提取子字符串

```
# 从第二个字符开始截取4个字符
string="0123456789"
echo ${string:1:4}	# 输出1234，第一个字符的索引值为0
```

- 查找子字符串

```
# 查找字符i或o的位置（哪个字母先出现就计算哪个）
string="runoob is a great site"
echo `expr index "$string" io`	# 输出4
```

注意：这里的expr index "\$string" io 不可以写成expr index \$string io，因为string中含有空格。

------

- shell 数组

bash支持一维数组（不支持多维数组），并且没有限定数组的大小。

数组元素的下标由0开始编号。

- 定义数组

在shell中，用括号来表示数组，数组元素用“空格”符号分隔开。定义数组的一般形式为：

```
array_name=(value0 value1 value2 value3)
或者
array_name=(
value0
value1
value2
value3
)
```

```
# 还可以定义数组的各个分量：
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```

**可以不使用连续的下标，而且下标的范围没有限制。**

- 读取数组

读取数组元素值的一般格式是：

```
${数组名[下标]}
```

使用@符号可以获取数组中的所有元素，例如：

```
echo ${array_name[@]}
```

- 获取数组的长度

获取数组长度的方法与获取字符串长度的方法相同，例如：

```
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
length_n=${#array_name[n]}
```

------

- shell注释

以`#`开头的行就是注释，会被解释器忽略。

多行注释：

```
:<<EOF
注释内容...
注释内容...
注释内容...
EOF
EOF 也可以使用其他符号:
:<<'
注释内容...
注释内容...
注释内容...
'

:<<!
注释内容...
注释内容...
注释内容...
!
```

#### 3.shell 传递参数

可以在执行shell脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n代表一个数字，1为执行脚本的第一个参数，2为执行脚本的第二个参数，...，以此类推。

```
#!/bin/bash
# author:liuying
# 向脚本传递2个参数并分别输出，其中$0为执行的文件名（包含文件路径）
echo "shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
```

另外，<span id = 'table' >还有几个特殊字符用来处理参数：</span>

| 参数处理                                                     | 说明                                                        |
| ------------------------------------------------------------ | ----------------------------------------------------------- |
| $#                                                           | 传递到脚本或函数的参数个数                                  |
| $*                                                           | 以一个单字符串显示所有向脚本传递的参数。                    |
| $$                                                           | 脚本运行的当前进程ID号                                      |
| $!                                                           | 后台运行的最后一个进程的ID号                                |
| $@       | 与$*相同，但是使用时加引号，并在引号中返回每个参数 |                                                             |
| $-                                                           | 显示shell使用的当前选项，与set命令功能相同                  |
| $?                                                           | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误 |

\$* 与 \$@ 区别：

a. 相同点：都是引用所有参数。

b. 不同点：只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，，则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。

#### 4. shell 数组

- 关联数组

bash支持关联数组，可以使用任意的字符串、或者整数作为下标来访问数组元素。

关联数组使用declare命令来声名，语法格式如下：

```
declare -A array_name
```

-A 选项就是用于声明一个关联数组。

关联数组的键是唯一的。

例如：

```
declare -A site=(["google"]="www.google.com" ["runoob"]="www.runoob.com" ["taobao"]="www.taobao.com")
```

也可以先声明一个关联数组，然后再设置键与值。

```
declare -A site
site["google"]="www.google.com"
site["runoob"]="www.runoob.com"
site["taobao"]="www.taobao.com"
```

访问关联数组元素可以使用指定的键，格式如下：

```
array_name["index"]
```

获取数组中的所有元素：

使用`@`或`*`。

```
echo ${array_name[@]}
echo ${array_name[*]}
```

在数组前加一个感叹号`!`可以获取数组的所有键。

```
declare -A site
site["google"]="www.google.com"
site["runoob"]="www.runoob.com"
site["taobao"]="www.taobao.com"

echo "数组的键为: ${!site[*]}"
echo "数组的键为: ${!site[@]}"
```

#### 5. shell 运算符

shell支持多种运算符，包括：算数运算符、关系运算符、布尔运算符、字符串运算符、文件测试运算符。

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如`awk`和`expr`。

expr是一款表达式计算工具，使用它能够完成表达式的求值操作。

```
#!/bin/bash
val=`expr 2 + 2`
echo "两数之和为：$val"
```

注意：表达式和运算符之间要有空格，必须写成2 + 2，而不是2+2；

​			完整的表达式要被\` \`包含。

- 算数运算符

假定变量a=10,b=20:

| 运算符 | 举例                 | 说明                                       |
| ------ | -------------------- | ------------------------------------------ |
| +      | \`expr \$a + \$b\`   | 加法                                       |
| -      | \`expr \$a - \$b\`   | 减法                                       |
| *      | \`expr \$a \\* \$b\` | 乘法                                       |
| /      | \`expr \$a / \$b\`   | 除法                                       |
| %      | \`expr \$a % \$b\`   | 取余                                       |
| =      | a=$b                 | 赋值                                       |
| ==     | [ \$a == \$b]        | 相等。用于比较两个数字，相同则返回true     |
| !=     | [ \$a != \$b]        | 不相等。用于比较两个数字，不相同则返回true |

注意：条件表达式要放在方括号之间，并且要有空格！！！**同时除了赋值运算符，其他的运算符和表达式之间必须要有空格！！！**另外乘号（*）之前必须加反斜杠（\）才能实现乘法运算！！！

- 关系运算符

关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

假定变量a=10,b=20:

| 运算符 | 说明                                               | 举例            |
| ------ | -------------------------------------------------- | --------------- |
| -eq    | 检测两个数是否相等，相等返回true                   | [ \$a -eq \$b ] |
| -ne    | 检测两个数是否不相等，不相等返回true               | [ \$a -ne \$b ] |
| -gt    | 检测左边的数是否大于右边的，如果是，则返回true     | [ \$a -gt \$b ] |
| -lt    | 检测左边的数是否小于右边的，如果是，则返回true     | [ \$a -lt \$b ] |
| -ge    | 检测左边的数是否大于等于右边的，如果是，则返回true | [ \$a -ge \$b ] |
| -le    | 检测左边的数是否等于右边的，如果是，则返回true     | [ \$a -le \$b ] |

- 布尔运算符

假定变量a=10,b=20

| 运算符 | 说明                                          | 举例                          |
| ------ | --------------------------------------------- | ----------------------------- |
| !      | 非运算，表达式为true则返回false，否则返回true | [ ! false ]                   |
| -o     | 或运算，有一个表达式为true则返回true          | [ \$a -lt 20 -o \$b -gt 100 ] |
| -a     | 与运算，两个表达式都为true才返回true          | [ \$a -lt 20 -a \$b -gt 100 ] |

- 逻辑运算符

| 运算符 | 说明      | 举例                               |
| ------ | --------- | ---------------------------------- |
| &&     | 逻辑的AND | [[ \$a -lt 100 && \$b -gt 100 ]]   |
| \|\|   | 逻辑的OR  | [[ \$a -lt 100 \|\| \$b -gt 100 ]] |

- 字符串运算符

假定变量a="abc"，b="efg":

| 运算符 | 说明                                     | 举例           |
| ------ | ---------------------------------------- | -------------- |
| =      | 检测两个字符串是否相等，相等返回true     | [ \$a = \$b ]  |
| !=     | 检测两个字符串是否不相等，不相等返回true | [ \$a != \$b ] |
| -z     | 检测字符串长度是否为0，为0返回true       | [ -z $a ]      |
| -n     | 检测字符串长度是否不为0，不为0返回true   | [ -n "$a" ]    |
| $      | 检测字符串是否不为空，不为空返回true     | [ $a ]         |

- 文件测试运算符

文件测试运算符用于检测Unix文件的各种属性。

| 操作符  | 说明                                                 | 举例         |
| ------- | ---------------------------------------------------- | ------------ |
| -b file | 检测文件是否是块设备文件                             | [ -b $file ] |
| -c file | 检测文件是否是字符设备文件                           | [ -c $file ] |
| -d file | 检测文件是否是目录                                   |              |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件） |              |
| -g file | 检测文件是否设置了SGID位                             |              |
| -k file | 检测文件是否设置了粘着位                             |              |
| -p file | 检测文件是否是有名管道                               |              |
| -u file | 检测文件是否设置了SUID位                             |              |
| -r file | 检测文件是否可读                                     |              |
| -w file | 检测文件是否可写                                     |              |
| -x file | 检测文件是否可执行                                   |              |
| -s file | 检测文件是否为空                                     |              |
| -e file | 检测文件（包括目录）是否存在                         |              |
|         |                                                      |              |

其他检查符：

`-S`:判断某文件是否socket。

`-L`:检测文件是否存在并且是一个符号链接。

#### 6. shell printf命令

printf 使用引用文本或空格分隔的参数。

printf 命令的语法：

```
printf  format-string  [arguments...]
```

参数说明：format-string: 为格式控制字符串

​				   arguments: 为参数列表。

```
# format-string为双引号
printf "%d %s\n" 1 "abc"

# 单引号与双引号效果一样
printf '%d %s\n' 1 "abc"

# 没有引号也可以输出
printf %s abcdef

# 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
printf %s abc def

printf "%s\n" abc def

printf "%s %s %s\n" a b c d e f g h i j

# 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替
printf "%s and %d \n"
```

#### 7. test命令

Shell中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。

```
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi
```

代码中的 **[]** 执行基本的算数运算，如：

```
#!/bin/bash

a=5
b=6

result=$[ a + b ] # 注意等号两边不能有空格
echo "result 为： $result"
```

#### 8. shell 流程控制

- if语句语法格式：

```
if condition
then
	command1
	command2
	...
	commandN
fi
```

写成一行（适用于终端命令提示符）：

```
if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi
```

查找指定进程格式：`ps -ef | grep 进程关键字`

- if else 语法格式：

```
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

- if else-if else 语法格式：

```
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

if else 的`[...]`判断语句中大于使用`-gt`，小于使用`-lt`。

如果使用`((...))`作为判断语句，大于和小于可以直接使用`>`和`<`。

```
if [ "$a" -gt "$b" ]; then
	...
fi
或者
if (( a > b )); then
	...
fi
```

- for 循环

for循环一般格式为：

```
for var in item1 item2 ... itemn
do
	command1
	command2
	...
	commandN
done
```

写成一行：

```
for var in item1 item2 ... itemN; do command1; command2... done;
```

当变量值在列表里，for循环即执行一次所有命令，使用变量名获取列表中的当前取值。命令可为任何有效的shell命令和语句。in列表可以包含替换、字符串和文件名。

in列表是可选的，如果不用它，for循环使用命令行的位置参数。

根据数组的长度输出数组：

```
#!/bin/bash
array=("beijing" "shanghai" "siyang")
for ((i=0; i<${#array[@]}; i++))
do
	echo ${array[$i]}
done
```



- while循环

while循环用于不断执行一系列命令，也用于从输入文件中读取数据。其语法格式为：

```
while condition
do
	command
done
```

例子：

```
#!/bin/bash
int=1
while (( $int <= 5 ))
do
	echo $int
	let "int++"
done
# bash中的let命令，其用于执行一个或多个表达式，变量计算中不需要加上$来表示变量
```

```
# while循环可用于读取键盘信息。
echo '按下<CTRL-D>退出'
echo -n '输入你最喜欢的网站名：'
while read FILM
do
	echo "是的！$FILM是一个好网站"
done
```

- 无限循环

无限循环语法格式：

```
while :
do
	command
done
或者
while true
do
	command
done
或者
for (( ; ; ))
```

- until 循环

until循环执行一系列命令直至条件为true时停止。

until循环与while循环在处理方式上刚好相反。

until语法格式：

```
until condition
do
	command
done
```

condition一般为条件表达式，如果返回值为false，则继续执行循环体内的语句，否则跳出循环。

- case ... esac

case...esac是一种多分支结构，每个case分支用右圆括号开始，用两个分号`;;`表示break，即执行结束，跳出整个case...esac作为结束标记。

```
case 值 in
模式1)
	command1
	command2
	...
	commandN
	;;
模式2)
	command1
	command2
	...
	commandN
	;;
esac
```

取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令。

- 跳出循环

break 命令允许跳出所有循环（终止执行后面的所有循环）。

```
#!/bin/bash
while:
do
	echo -n "输入1到5之间的数字："
	read aNum
	case $aNum in
		1|2|3|4) echo "你输入的数字为$aNum!"
		;;
		*) echo "你输入的数字不是1到5之间的！"
			break
		;;
	exac
done		
```

continue不会跳出所有循环，仅仅跳出当前循环。

```
#!/bin/bash
while :
do
	echo -n "输入1到5之间的数字："
	read aNum
	case $aNum in
		1|2|3|4|5) echo "你输入的数字为 $aNum !"
		;;
		*) echo "你输入的数字不是1到5之间的！"
			contine
			echo "游戏结束"
		;;
	esac
done
```

#### 9. shell函数

linux shell可以用户定义函数，然后在shell脚本中可以随便调用。

shell中函数的定义格式如下：

```
[ function ] funname [()]
{
	action;
	[return int;]
}
```

说明：

1. 可以带function fun()定义，也可以直接fun()定义，不带任何参数。
2.  参数返回，可以显示加：return返回，如果不加，将以最后一条命令运行结果作为返回值。return后跟数值n(0-255)

```
#!/bin/bash
demoFun(){
	echo "这是我的第一个shell函数！"
}
echo "—————————函数开始执行—————————"
demoFun
echo "—————————函数执行完毕—————————"
```

```
funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```

```
$(())和$[]等价
更多相关信息：
https://blog.csdn.net/hanjinjuan/article/details/119086556
```

注意：函数返回值在调用该函数后通过`$?`来获得。

​			所有函数在使用前必须定义。

- 函数参数

在shell中，调用函数时可以向其传递参数。在函数体内部，通过`%n`的形式来获取参数的值，例如，`$1`表示第一个参数，`$2`表示第二个参数，...

```
#!/bin/bash
fun(){
	echo "第一个参数为 $1"
	echo "第二个参数为 $2"
    echo "第二个参数为 ${2}"
	echo "第十个参数为 $10"
	echo "第十个参数为 ${10}"
	echo "参数总数有 $# 个！"
	echo "作为一个字符串输出所有参数 $* !"
}
fun 1 2 3 4 5 6 7 8 9 10 11 12
```

注意，`$10`不能获取第十个参数，获取第十个参数需要`${10}`。当n>=10时，需要使用`${n}`来获取参数。

​		`$?`仅对其上一条指令负责，一旦函数返回后其返回值没有立即保存入参数，那么其返回值将不再能通过`$?`获得。

​		函数与命令的执行结果可以作为条件语句使用。要注意的是，和 C 语言不同，shell 语言中 0 代表 true，0 以外的值代表 false。



另外，[还有几个特殊字符用来处理参数](#table)

#### 10. shell 输入/输出重定向

重定向命令列表：

| 命令            | 说明                                          |
| --------------- | --------------------------------------------- |
| command > file  | 将输出重定向到file                            |
| command < file  | 将输入重定向到file                            |
| command >> file | 将输出以追加的方式重定向到file                |
| n > file        | 将文件描述符为n的文件重定向到file             |
| n >> file       | 将文件描述符为n的文件以追加的方式重定向到file |
| n >& m          | 将输出文件m和n合并                            |
| n <& m          | 将输入文件m和n合并                            |
| << tag          | 将开始标记tag和结束标记tag之间的内容作为输入  |

注意：文件描述符0通常是标准输入（STDIN），1是标准输出（STDOUT），2是标准错误输出（STDERR）。

- 输出重定向

- 输入重定向

`command1 < file1`  这样，本来需要从键盘获取输入的命令会转移到文件读取内容。

```
# 同时替换输入和输出，执行command1，从文件infile读取内容，然后将输出写入到outfile中。
command1 < infile > outfile
```

- 重定向深入讲解

一般情况下，每个unix/linux命令运行时都会打开三个文件：

1. 标准输入文件(stdin)：stdin的文件描述符为0，unix程序默认从stdin读取数据。
2. 标准输出文件(stdout)：stdout的文件描述符为2，unix程序默认向stdout输出数据。
3. 标准错误文件(stderr)：stderr的文件描述符为2，unix程序会向stderr流中写入错误信息。

默认情况下，command > file 将stdout重定向到file，command < file将stdin重定向到file。

如果希望stderr重定向到file，可以写：

```
command 2> file
```

如果希望stderr追加到file文件末尾，可以写：

```
command 2>> file
```

如果希望将stdout和stderr合并后重定向到file，可以写：

```
command > file 2>&1
或者
command >> file 2>&1
```

如果希望对stdin和stdout都重定向，可以写：

```
# command命令将stdin重定向到file1，将stdout重定向到file2
command < file1 > file2
```

- Here Document

Here Document是shell中的一种特殊的重定向方式，用来将输入重定向到一个交互式shell脚本或程序。

其基本的形式如下：

```
command << delimiter
	document
delimiter
```

它的作用是将两个delimiter之间的内容(document)作为输入传递给command。

```
# 例子
wc -l << EOF
	liuying
	刘颖
EOF

cat << EOF
刘颖
EOF
```

- /dev/null文件

如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到/dev/null:

```
command > /dev/null
```

/dev/null是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。将命令的输出重定向到它，会起到“禁止输出”的结果。

如果希望屏蔽stdout和stderr，可以写：

```
command > /dev/null 2>&1
```

#### 11. shell 文件包含

shell可以包含外部脚本。这样可以很方便的封装一些公用的代码作为一个独立的文件。

shell文件包含的语法格式如下：

```
. filename	#注意(.)和文件名之间有一个空格
或
source filename
```



#### 附录

一些关键字：

`readonly 命令`

`case 命令`

`while 命令`

`for 命令`

`until 命令`

`unset 命令`

`echo 命令`

`expr 命令`

`awk 命令`

`expr 命令`

`read 命令`

`date 命令`

`printf 命令`

`test 命令`

`sleep 命令`

`let 命令`

`cat 命令`

`wc 命令`



Shell 中的特殊字符：https://www.runoob.com/w3cnote/shell-special-char.html

