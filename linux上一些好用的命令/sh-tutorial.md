## 一、什么是shell？
- Shell是一个用 C 语言编写的程序
   - 它是用户使用 Linux 的桥梁。
   - Shell 既是一种命令语言
   - 又是一种程序设计语言。
- Shell 是指一种应用程序
   - 这个应用程序提供了一个界面
   - 用户通过这个界面访问操作系统内核的服务
## 二、shell的分类
### 1.Linux的shell

- 在linux中有很多类型的shell，不同的shell具备不同的功能，shell还决定了脚本中函数的语法
- Linux中默认的shell是 /bash/bash( 重点 )
- 流行的shell有**ash、bash、ksh、csh、zsh**等，不同的shell都有自己的特点以及用途
### 2.编写规范
> #!/bin/bash	[指定告知系统当前这个脚本要使用的shell解释器]Shell相关指令

### 3.文件命名规范

- 文件名.sh     **.sh**是linux下bash shell 的默认后缀
### 4.Bash常用快捷键
| ctrl+A | 把光标移动到命令行开头。如果我们输入的命令过长，想要把光标移动到命令行开头时使用。 |  |
| --- | --- | --- |
| ctrl+E | 把光标移动到命令行结尾 |  |
| ctrl+C | 强制终止当前的命令 |  |
| ctrl+L | 清屏，相当于clear命令 |  |
| ctrl+U | 删除或剪切光标之前的命令。我输入了一行很长的命令，不用使用退格键一个一个字符的删除，使用这个快捷键会更加方便 |  |
| ctrl+K | 删除或剪切光标之后的内容 |  |
| ctrl+Y | 粘贴ctrl+U或ctul+K剪切的内容 |  |
| ctrl+R | 在历史命令中搜索，按下ctrl+R之后，就会出现搜索界面，只要输入搜索内容，就会从历史命令中搜索 |  |
| ctrl+D | 退出当前终端 |  |
| ctrl+Z | 暂停，并放入后台。这个快捷键牵扯工作管理的内容，我们在系统管理章节详细介绍 |  |
| ctrl+S | 暂停屏幕输出 |  |
| ctrl+Q | 恢复屏幕输出 |  |

### 5.输入输出重定向
#### 5.1.linux 的标准输入与输出
| **设备** | **设备名** | **文件描述符** | **类型** |
| --- | --- | --- | --- |
| 键盘 | /dev/stdin | 0 | 标准输入 |
| 显示器 | /dev/stdout | 1 | 标准输出 |
| 显示器 | /dev/stderr | 2 | 标准错误输出 |

#### 5.2.输入重定向

- 是指不使用系统提供的标准输入端口，而进行重新的指定
- 换言之，输入重定向就是不使用标准输入端口输入文件，而是使用指定的文件作为标准输入设备
- 重定向简单理解就是使用 “<”符来修改标准输入设备
| **类型** | **符号（语法）** | **功能** |
| --- | --- | --- |
| 标准输入 | 命令<文件1 | 命令把文件1的内容作为标准输入设备 |
| 标识符限定输入 | 命令<<标识符 | 命令把标准输入中读入内容，直到遇到“标识符”分解符为止 |
| 输入输出重定向（同时使用） | 命令< 文件1 >文件2 | 命令把文件1的内容作为标准输入，把文件2作为标准输出。 |

#### 5.3.输出重定向

- 通俗的讲，重定向输出就是把要输出的文件信息写入到一个文件中去，而不是将要输出的文件信息输出到控制台（显示屏）
- 在linux中，默认的标准输出设备是控制台（或称为显示器）,用户输出的信息默认情况下都会显示到控制台
- **&表示全部文件，文件不管对错，1表示标准输出文件，2表示标准错误输出**
| **类型** | **符号** | **作用** |
| --- | --- | --- |
| 标住输出重定向 | 命令 > 文件 | 以覆盖方式，把命令的正确输出内容输出到指定的文件或设备当中 |
| 标住输出重定向 | 命令 >> 文件 | 以追加方式，把命令的正确输出内容输出到指定的文件或设备当中 |
| 标准错误输出重定向 | 错误命令2 > 文件 | 以覆盖方式，把命令的错误输出输出到指定的文件或设备当中 |
| 标准错误输出重定向 | 错误命令2 >> 文件 | 以追加方式，把命令的错误输出输出到指定的文件或设备当中 |
| 正确输出和错误输出同时保存 | 命令 > 文件 2>&1 | 以覆盖的方式，把正确输出和错误输出都保存到同一个文件当中 |
| 正确输出和错误输出同时保存 | 命令 >> 文件 2>&1 | 以追加的方式，把正确输出和错误输出都保存到同一个文件当中 |
| 正确输出和错误输出同时保存 | 命令 &> 文件 | 以覆盖的方式，把正确输出和错误输出都保存到同一个文件当中 |
| 正确输出和错误输出同时保存 | 命令 &>> 文件 | 以追加的方式，把正确输出和错误输出都保存到同一个文件当中 |
| 正确输出和错误输出同时保存 | 命令 >> 文件1 2>>文件2 | 把正确的输出追加到文件1中，把错误的输出追加到文件2中 |

#### 5.4./dev/null 文件

- 如果希望执行某个命令，但又不希望在屏幕上显示输出结果
- 那么可以将输出重定向到**/dev/null**中
> [root@localhost ~]$  command > dev/null

### 6.多命令顺序执行
| **多命令执行符** | **作用** | **格式** |
| --- | --- | --- |
| ； | 命令1 ；命令2 | 多个命令顺序执行，命令之间没有任何逻辑联系 |
| && | 命令1 && 命令2 | 与 |
| ll | 命令1 &#124;&#124; 命令2 | 或 |

### 7.shell脚本的执行
```bash
[root@localhost ~]$ vim test.sh
#!/bin/bash
echo “hello world”
```
#### 7.1.两种方式执行shell脚本
###### 7.1.1.给文件增加执行权限
```bash
[root@localhost ~]$ chmod u+x test.sh
[root@localhost ~]$ ./test.sh  #绝对路径或相对路径执行
```
###### 7.1.2.通过Bash调用执行脚本
```bash
[root@localhost ~]$ bash test.sh
```

## 三、shell变量
### 1.变量的命名规则
#### 1.1遵守的规则 

   - 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头。
   - 等号左右两侧不能有空格，可以使用下划线“_”，变量的值如果有空格，需要使用单引号或双引号包括。如:“test=“hello world!””。其中双引号括起来的内容“$”，“(”和反引号都拥有特殊含义，而单引号括起来的内容都是普通字符。
   - 不能使用标点符号，不能使用bash里的关键字（可用help命令查看保留关键字）
   - 环境变量建议大写，便于区分
   - 如果需要增加变量的值，那么可以进行变量值的叠加。不过变量需要用双引号包含"$变量名"或用${变量名}包含变量名
```bash
[root@localhost ~]$ test=123
[root@localhost ~]$ test="$test"456
[root@localhost ~]$ echo $test
123456
#叠加变量test，变量值变成了123456
[root@localhost ~]$ test=${test}789
[root@localhost ~]$ echo $test
123456789
#再叠加变量test，变量值编程了123456789
```
#### 1.2注意

   - **关于单双引号的问题：**
   - **双引号能够识别变量，双引号能够实现转义（类似于“\*”）**
   - **单引号是不能识别变量，只会原样输出，单引号是不能转义的**
#### 1.3shell中特殊符号
| 符号 | 作用 |
| --- | --- |
| ’’ | 单引号。在单引号中所有的特殊符号，如“$”和”(反引号)都没有特殊含义。单引号括起来的都是普通字符，会原样输出 |
| “ ” | 双引号。在双引号中特殊符号都没有特殊含义，但是“$”，“`”（esc键下面）和“\\”是例外，拥有“调用变量的值”、“引用命令”和“转义符”的特殊含义。 |
| · · | 反引号。反引号括起来的内容是系统命令，在Bash中会先执行它。和( ) 作用一样 ，不过推荐使用 ()作用一样，不过推荐使用()作用一样，不过推荐使用()，因为反引号非常容易看错 |
| $( ) | 和反引号作用一样，用来引用系统命令。(推荐使用) |
| ( ) | 用于一串命令执行时，()中的命令会在子Shell中运行 |
| { } | 用于一串命令执行时，{ }中的命令会在当前Shell中执行。也可以用于变量变形与替换 |
| [  ] | 用于变量的测试 |
| # | 在Shell脚本中，#开头的行代表注释 |
| $ | 用于调用变量的值，如需要调用变量name的值时，需要用$name的方式得到变量的值 |
| \\ | 转义符，跟在\\之后的特殊符号将失去特殊含义，变为普通字符。如$将输出“$”符号，而不当做是变量引用 |

#### 1.4单引号和双引号
```bash
[root@localhost ~]$ name=sc
#定义变量name 的值是sc（就是最正直的人，超哥我了!）
[root@localhost ~]$ echo '$name'
$name
#如果输出时使用单引号，则$name原封不动的输出
[root@localhost ~]$ echo "$name"
sc
#如果输出时使用双引号，则会输出变量name的值 sc

[root@localhost ~]$ echo `date`
2018年10月21日星期一18:16:33 CST
#反引号括起来的命令会正常执行
[root@localhost ~]$ echo '`date`'
`date`
#但是如果反引号命令被单引号括起来，那么这个命令不会执行，―date`会被当成普通字符输出
[root@localhost ~]$ echo "`date'"
2018年10月21日星期一18:14:21 CST
#如果是双引号括起来，那么这个命令又会正常执行
```
#### 1.5反引号
```bash
[root@localhost ~]$ echo ls
ls
#如果命令不用反引号包含，命令不会执行，而是直接输出
[root@localhost ~]$ echo `ls`
anaconda-ks.cfginstall.loginstall.log.syslog sh test testfile
#只有用反引号包括命令，这个命令才会执行
[root@localhost ~]$ echo $(date)
2018年10月21日星期一18:25:09 CST
#使用$(命令)的方式也是可以的
```
### 2.变量的分类
#### 2.1用户自定义变量:

- 这种变量是最常见的变量，由用户自由定义变量名和变量的值
```bash
[root@localhost ~]$ 2name="shen chao"
-bash: 2name=shen chao: command not found
#变量名不能用数字开头
[root@localhost ~]$ name = "shenchao"
-bash: name: command not found
#等号左右两侧不能有空格
[root@localhost ~]$ name=shen chao
-bash: chao: command not found
#变量的值如果有空格，必须用引号包含
```
```bash
[root@localhost ~]$ name="shen chao"
#定义变量name
[root@localhost ~]$ echo $name #调用变量使用  $变量名
shen chao
#输出变量name的值
```
```bash
[root@localhost ~]$ set [选项]
选项:
-u:如果设定此选项，调用未声明变量时会报错（默认无任何提示）
-x:如果设定此选项，在命令执行之前，会把命令先输出一次
+<参数> :取消某个set曾启动的参数。

[root@localhost ~]$ set
BASH=/bin/bash
…省略部分输出…
name='shen chao'
#直接使用set 命令，会查询系统中所有的变量，包含用户自定义变量和环境变量
[root@localhost ~]$ set -u
[root@localhost ~]$ echo $file
-bash: file: unbound variable
#当设置了-u选项后，如果调用没有设定的变量会有报错。默认是没有任何输出的。
[root@localhost ~]$ set -x
[root@localhost ~]$ ls
+ls --color=auto
anaconda-ks.cfginstall.loginstall.log.syslog sh tdir testtestfile
#如果设定了-x选项，会在每个命令执行之前，先把命令输出一次

[root@localhost ~]$ set +x
#取消启动的x参数
```
```bash
[root@localhost ~]$ unset 变量名
```
#### 2.2环境变量:

- 这种变量中主要保存的是和系统操作环境相关的数据，比如当前登录用户，用户的家目录，命令的提示符等。不是太好理解吧，那么大家还记得在Windows中，同一台电脑可以有多个用户登录，而且每个用户都可以定义自己的桌面样式和分辨率，这些其实就是Windows的操作环境，可以当做是Windows的环境变量来理解。环境变量的变量名可以自由定义，但是一般对系统起作用的环境变量的变量名是系统预先设定好的
```bash
[root@localhost ~]$  export age="18"
#使用export声明的变量即是环境变量
```
```bash
env命令和set命令的区别：
set命令可以查看所有变量，而env命令只能查看环境变量。
[root@localhost ~]$ unset gender   #删除环境变量gender
[root@localhost ~]$ env | grep gender
```
```bash
[root@localhost ~]$ env
HOSTNAME=localhost.localdomain      #主机名
SHELL=/bin/bash                     #当前的shell
TERM=linux                          #终端环境
HISTSIZE=1000                       #历史命令条数
SSH_CLIENT=192.168.4.1594824 22     #当前操作环境是用ssh连接的，这里记录客户端ip
SSH_TTY=/dev/pts/1                  #ssh连接的终端时pts/1
USER=root                           #当前登录的用户
..........更多参数可以使用set和env命令查看.............
```
#### 2.3位置参数变量:

-  这种变量主要是用来向脚本当中传递参数或数据的，变量名不能自定义，变量作用是固定的
```bash
位置参数变量	作用
$n	n为数字，$0表示当前 Shell 脚本程序的名称，$1-9 代 表 第 一 到 第 九 个 参 数 , 十 以 上 的 参 数 需 要 用 大 括 号 包 含 ， 如 9代表第一到第九个参数,十以上的参数需要用大括号包含，如9代表第一到第九个参数,十以上的参数需要用大括号包含，如{10}
$*	这个变量代表命令行中所有的参数，$把所有的参数看成一个整体
$@	这个变量也代表命令行中所有的参数，不过$@把每个参数区分对待
$#	这个变量代表命令行中所有参数的个数
```

- **$1 是你给你写的shell脚本传的第一个参数，$2 是你给你写的shell脚本传的第二个参数…**
```bash
[root@localhost sh]$ vim test.sh
#!/bin/sh
echo "shell脚本本身的名字: $0"
echo "传给shell的第一个参数: $1"
echo "传给shell的第二个参数: $2"
```
保存退出后，你在Test.sh所在的目录下输入 bash Test.sh 1 2
结果输出：
```bash
shell脚本本身的名字: Test.sh
传给shell的第一个参数: 1
传给shell的第二个参数: 2
```

- **$*会把接收的所有参数当成一个整体对待，而$@则会区分对待接收到的所有参数。举个例子:**
```bash
[root@localhost sh]$ vi parameter2.sh
#!/bin/bash
for i in"$*"
#定义for循环，in后面有几个值，for会循环多少次，注意“$*”要用双引号括起来
#每次循环会把in后面的值赋予变量i
#Shell把$*中的所有参数看成是一个整体，所以这个for循环只会循环一次
	do
		echo "The parameters is: $i"
		#打印变量$i的值
	done
x=1
#定义变量x的值为1
for y in"$@"
#同样in后面的有几个值，for循环几次，每次都把值赋予变量y
#可是Shel1中把“$@”中的每个参数都看成是独立的，所以“$@”中有几个参数，就会循环几次
	do
		echo "The parameter$x is: $y"
		#输出变量y的值
		x=$(( $x +1 ))
		#然变量x每次循环都加1，为了输出时看的更清楚
	done
```
#### 2.4预定义变量: 

- 是Bash中已经定义好的变量，变量名不能自定义，变量作用也是固定的
| **预定义变量** | **作用** |
| --- | --- |
| $? | 最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行;如果这个变量的值为非О(具体是哪个数，由命令自己来决定），则证明上一个命令执行不正确了。 |
| $$ | 当前进程的进程号（PID) |
| $! | 后台运行的最后一个进程的进程号(PID) |

- **先来看看”$?”这个变量，举个例子说明**
```bash
[root@localhost sh]$ ls
count.sh hello.sh parameter2.sh parameter.sh
#ls命令正确执行
[root@localhost sh]$ echo $?
#预定义变量“$?”的值是0，证明上一个命令执行正确
[root@localhost sh]$ ls install.log
ls:无法访问install.log:没有那个文件或目录
#当前目录中没有install.log文件，所以ls命令报错了
[root@localhost sh]$ echo $?
2
#变量“$?”返回一个非О的值，证明上一个命令没有正确执行
#至于错误的返回值到底是多少，是在编写ls命令时定义好的，如果碰到文件不存在就返回数值2
```

- **再来说明下”$$”和”$!”这两个预定义变量**
```bash
[root@localhost sh]$ vi variable.sh
#!/bin/bash
echo "The current process is $$"
#输出当前进程的PID.
#这个PID就是variable.sh这个脚本执行时，生成的进程的PID
find /root -name hello.sh &
#使用find命令在root目录下查找hello.sh文件
#符号&的意思是把命令放入后台执行，工作管理我们在系统管理章节会详细介绍
echo "The last one Daemon process is $!"
#输出这个后台执行命令的进程的PID，也就是输出find命令的PID号
```
#### 2.5表格对比
| **变量分类** | **名称** | **作用** | **内容** |
| --- | --- | --- | --- |
| 用户自定义变量 | 自定义 | 自定义 | 自定义 |
| 用户自定义环境变量 | 自定义 | 自定义 | 自定义 |
| 系统自带环境变量(/etc/profile) | 确定 | 确定 | 自定义 |
| 位置参数变量 | 确定 | 自定义 | 自定义 |
| 预定义变量 | 确定 | 自定义 | 自定义 |

**2.6只读变量**
```bash

[root@localhost sh]$ vi readonly.sh
#!/bin/bash
a=10
#语法：readonly 变量名
readonly a
a=20   #会报错readonly variable
echo $a
```
#### 2.7接受键盘输入
```bash
[root@localhost ~]$ read [选项][变量名]
选项:
	-a 后跟一个变量，该变量会被认为是个数组，然后给其赋值，默认是以空格为分割符。
	-p： “提示信息”：在等待read输入时，输出提示信息
	-t： 秒数：read命令会一直等待用户输入，使用此选项可以指定等待时间
	-n： 数字：read命令只接受指定的字符数，就会执行
	-s： 隐藏输入的数据，适用于机密信息的输入
    -d： 后面跟一个标志符，其实只有其后的第一个字符有用，作为结束的标志。
    -e： 在输入的时候可以使用命令补全功能。
变量名:
变量名可以自定义，如果不指定变量名，会把输入保存入默认变量REPLY.
如果只提供了一个变量名，则整个输入行赋予该变量.
如果提供了一个以上的变量名，则输入行分为若干字，一个接一个地赋予各个变量，而命令行上的最后一个变量取得剩余的所有字
```
#### 2.8简单的例子说明read命令f
```bash
[root@localhost sh]$ vi read.sh
#!/bin/bash

read -t 30 -p "Please input your name: " name
#提示“请输入姓名”并等待30 秒，把用户的输入保存入变量name 中
echo "Name is $name"
#看看变量“$name”中是否保存了你的输入

read -s -t 30 -p "Please enter your age: " age
#提示“请输入年龄”并等待30秒，把用户的输入保存入变量age中
#年龄是隐私，所以我们用“-s”选项隐藏输入
echo -e "\n"
#调整输出格式，如果不输出换行，一会的年龄输出不会换行
echo "Age is $age"

read -n 1 -t 30 -p "Please select your gender[M/F]:" gender
#提示“请选择性别”并等待30秒，把用户的输入保存入变量gender
#使用“-n1”选项只接收一个输入字符就会执行（都不用输入回车）
echo -e "\n"
echo "Sex is $gender"

```

## 四、shell运算符

- 在shell中，运算符和其他编程脚本语言一样，常见的有算数运算符、关系运算符、逻辑运算符、字符串运算符、文件测试运算符等
### 1.算数运算符

- 原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 **awk 和 expr，expr** 最常用。
#### 1.1expr 是一款表达式计算工具，使用它能完成表达式的求值操作

   - 例如，两个数相加(注意使用的是反引号 ` 而不是单引号 ')
```bash
[root@localhost ~]$ vi computer.sh
#!/bin/bash
val=`expr 2 + 2`
echo "两数之和为 : $val"
#注意
#表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程语言不一样。
#完整的表达式要被 ` ` 包含，注意这个字符不是常用的单引号，在 Esc 键下边。
```
#### 1.2下表列出了常用的算术运算符，假定变量 a 为 10，变量 b 为 20
| 运算符 | 说明 | 举例 |
| --- | --- | --- |
| + | 加法 |   expr $a + $b  |
| - | 减法 |   expr $a - $b |
| * | 乘法 |   expr $a \\* $b |
| / | 除法 |   expr $a \\* $b |
| % | 取余 |   expr $b / $a |
| = | 赋值 |   expr $b % $a |
| == | 相等 | `[$a == $b] ` |
| ！= | 不相等 | `[$a == $b]` |

#### 1.3算数运算符举例
```bash
[root@localhost ~]$ vi computers.sh
#!/bin/bash
a=10
b=20
echo ' '
echo 'a+b= ' `expr $a + $b`
echo 'a-b= ' `expr $a - $b`
echo 'a*b= ' `expr $a \* $b`
echo 'a/b= ' `expr $a / $b`
echo 'a%b= ' `expr $a % $b`

#判断是否相等
if [ $a == $b ]
then
	echo 'a等于b'
else
	echo 'a不等于b'
fi

```
### 2.关系运算符
#### 2.1六种关系运算

- 关系运算符只支持数字，不支持字符串，除非字符串的值是数字。
| 运算符 | 单词 | 说明 | 举例 |
| --- | --- | --- | --- |
| -eq | equal | 检测两个数是否相等，相等返回 true | [ $a -eq $b ] 返回 false |
| -ne | not equal | 检测两个数是否相等，不相等返回 true | [ $a -ne $b ] 返回 true |
| -gt | great than | 检测左边的数是否大于右边的，如果是，则返回 true | [ $a -gt $b ] 返回 false |
| -lt | less than | 检测左边的数是否小于右边的，如果是，则返回 true | [ $a -lt $b ] 返回 true |
| -ge | great than or equal | 检测左边的数是否大于等于右边的，如果是，则返回 true | [ $a -ge $b ] 返回 false |
| -ge | less than or equal | 检测左边的数是否小于等于右边的，如果是，则返回 true | [ $a -le $b ] 返回 true |

```bash
[root@localhost ~]$ [ 10 -gt 10 ] 
[root@localhost ~]$ echo $? 
 1
[root@localhost ~]$ [ 10 -eq 10 ] 
[root@localhost ~]$ echo $? 
 0
```
#### 2.2关系运算举例

- **案例：判断当前输入的用户是否存在。如果存在则提示“用户存在”否则提示“用户不存在”。**
- **如果要在shell脚本使用linux命令，可以使用$()包裹命令**
**例如：disk_size=$(df -h | awk ‘NR==2 {print $5}’)**
```bash
[root@localhost ~]$ vim demo.sh 
#!/bin/bash
#接受用户的输入
read -p '请输入需要查询的用户名:' username

#获取指定用户名在passwd文件中出现的次数
count=$(cat /etc/passwd | grep $username | wc -l)
#count=`cat /etc/passwd | grep $username | wc -l`

#判断出现的次数，如果次数=0则用户不存在，反之存在
if [  $count == 0 ]
then 
		echo '用户不存在'
	else 
		echo '用户存在'
fi
```
### 3. 逻辑运算符

- **下表列出了常用的布尔运算符，假定变量 a 为 10，变量 b 为 20：**
| 运算符 | 说明 | 举例 |
| --- | --- | --- |
| ! | 非运算，表达式为 true 则返回 false，否则返回 true | [ ! false ] 返回 true |
| -o | 或（或者）运算，有一个表达式为 true 则返回 true | [ $a -lt 20 -o $b -gt 100 ] 返回 true |
| -a | 与（并且）运算，两个表达式都为 true 才返回 true | [ $a -lt 20 -a $b -gt 100 ] 返回 false |

- **或运算：一个为真即为真，全部为假才是假**
- **与运算：一个为假即为假，全部为真才是真**
### 4. 字符串运算符

- **下表列出了常用的字符串运算符，假定变量 a 为 “abc”，变量 b 为 “efg”：**
| 运算符 | 说明 | 举例 |
| --- | --- | --- |
| = | 检测两个字符串是否相等，相等返回 true | [ $a = $b ] 返回 false |
| != | 检测两个字符串是否相等，不相等返回 true | [ $a != $b ] 返回 true |
| -z | 检测字符串长度是否为0，为0返回 true | [ -z $a ] 返回 false |
| -n | 检测字符串长度是否为0，不为0返回 true | [ -n $a ] 返回 true |
| str | 检测字符串是否为空，不为空返回 true | [ $a ] 返回 true |

### 5. 文件测试运算符（重点）

- **文件测试运算符用于检测 Unix/Linux 文件的各种属性。**

