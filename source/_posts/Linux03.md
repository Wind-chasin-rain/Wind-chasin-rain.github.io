---
title: Linux学习03
date: 2023-07-19 18:07:55
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191828815.jpg
tags:
  - Linux
  - Shell
updated:
categories: 学习笔记
description: Linux基础知识，Shell编程，日志管理，备份与恢复
mathjax:
toc: true
toc_number: true
---

# Shell编程

## Shell是什么

Shell是一个命令行解释器，它为用户提供了一个向Linux内核发送请求以便运行程序的界面系统级程序，用户可以用Shell来启动、挂起、停止甚至是编写一些程序。d&kF%Nfti3s-

## Shell脚本的执行方式

### 脚本格式要求

1.脚本以#!bin/bash开头（表明以bashell执行）

2.脚本需要有可执行权限

3.shell脚本一般以.sh结尾。虽然也可以以别的结尾，但是一般约定俗成以.sh结尾。

<br>

### 脚本的常用执行方式

**编写第一个Shell脚本**

需求说明：创建一个Shell脚本，输出hello,world~

方式1（输入脚本的绝对路径或相对路径）

说明：首先要赋予helloworld.sh脚本的+x权限，再执行脚本

```bash
mkdir /root/shcode # 创建shcode目录，用于存放后续的shell脚本
cd /root/shcode/
vim hello.sh	   # 第一行输入：#!/bin/bash 第二行输入：echo "hello,world~"
chmod u+x hello.sh # 给所有者增加执行权限
./hello.sh		   # 或使用绝对路径执行脚本：/root/shcode/hello.sh
12345
```

方法2（sh+脚本）

说明：不用赋予脚本+x权限，直接执行即可。

```bash
chmod u-x hello.sh # 去掉执行权限
sh hello.sh		   # 将hello.sh当脚本执行，此时不需要执行权限
```

<br>

## Shell的变量

### Shell变量介绍

1.Linux Shell中的变量分为：系统变量和用户自定义变量

2.系统变量：\$HOME, \$PWD, \$SHELL, \$USER等等，比如：echo $HOME等等。

3.显示当前shell中所有变量：set

<br>

### shell变量的定义

基本语法

1.定义变量：变量名=值（不要打空格）

2.撤销变量：unset 变量

3.声明静态变量：readonly 变量，注意：不能unset

快速入门

案例1：定义变量A

案例2：撤销变量A

案例3：声明静态的变量B=2，不能unset（静态变量不会被反复定义和初始化，只会被定义一次)

案例4：可把变量提升为全局环境变量，可供其他shell程序使用

```bash
#!/bin/bash
# 案例1：定义变量A
A=100
# 定义变量不需要$，输出变量需要$
echo A=$A
echo "A=$A"
# 案例2：撤销变量A
unset A
echo "A=$A" # 此时只会输出A= ，因为变量A已经被销毁了
# 案例3：声明静态的变量B=2，不能unset
readonly B=2
echo "B=$B"
unset B     
```

<br>

### shell变量的命名和赋值规则

定义变量的规则

1.变量名称可以由字母、数字和下划线组成，但是不能以数字开头。

5A=200(不可以)

2.等号两侧不能有空格。

3.变量名称一般习惯为大写

将命令的返回值赋给变量

1.A=``反引号，运行里面的命令，并把结果返回给变量A（没有反引号，会认为是单词赋给A）

2.A=$(date)等价于反引号

<br>

## 设置环境变量

**基本语法**

1.export 变量名=变量值（功能描述：将shell变量输出为环境变量/全局变量）

2.source 配置文件 （功能描述：让修改后的配置信息立即生效）

3.echo $变量名 （功能描述：查询环境变量的值）

**快速入门**

1.在/etc/profile文件中定义TOMCAT_HOME环境变量

2.查看环境变量TOMCAT_HOME的值

3.在另外一个shell程序中使用TOMCAT_HOME

```bash
vim /etc/profile
# 在/etc/profile文件最后添加
export TOMCAT_HOME=/opt/tomcat
# wq保存退出
source /etc/profile # 刷一下profile文件，让配置信息生效
echo $TOMCAT_HOME   # 返回/opt/tomcat
```

注意：在输出TOMCAT_HOME环境变量前，需要让其生效source/etc/profile

补充：shell脚本的多行注释

```shell
:<<! 
内容
! 
```

<br>

## 位置参数变量

当我们执行一个shell脚本时，如果希望获取到命令行的参数信息，就可以使用到位置参数变量

比如：./myshell.sh 100 200，这个就是一个执行shell的命令行，可以在myshell脚本中获取到参数信息

**基本语法**

![image-20230717142522210](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827292.png)

案例

案例：编写一个shell脚本myshell.sh，在脚本中获取到命令行的各个参数信息。

```bash
#!/bin/bash
echo "0=$0, 1=$1, 2=$2"
echo "所有的参数=$*"
echo "$@"
echo "参数的个数=$#"
```

<br>

## 预定义变量

就是shell设计者事先已经定义好的变量，可以直接在shell脚本中使用

**基本语法**

$$ （功能描述：当前进程的进程号(PID))

$! （功能描述：后台运行的最后一个进程的进程号（PID））

$? （功能描述：最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行；如果这个变量的值为非0（具体是哪个数，由命令自己来决定），则证明上一个命令执行不正确了。）

**应用实例：**

在一个shell脚本中简单使用一下预定义变量preVar.sh

```bash
#!/bin/bash
echo "当前执行的进程id=$$"
# 以后台的方式运行一个脚本(后面带个&)，并获取他的进程号
/root/shcode/myshell.sh &
echo "最后一个后台方式运行的进程id=$!"
echo "执行的结果=$?"
```

<br>

## 运算符

**基本语法**

![image-20230717143610960](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827294.png)

**应用实例oper.sh**

案例1：计算(2+3)*4的值

案例2：请求出命令行两个参数[整数]的和，比如20和50

```bash
#!/bin/bash
# 案例1：计算（2+3）X4的值
RES1=$(((2+3)*4))
echo "res1=$RES1"
# 使用第二种方式，推荐使用
RES2=$[(2+3)*4]
echo "res2=$RES2"
# 使用第三种方式 expr
TEMP=`expr 2 + 3`
RES3=`expr $TEMP \* 4`
echo "temp=$TEMP"
echo "res3=$RES3"
# 案例2：请求出命令行的两个参数[整数]的和 20 50
SUM=$[$1+$2]
echo "sum=$SUM"
```

<br>

## 条件判断

**基本语法**

```bash
if [ condition ] # 注意condition前后要有空格
then
	...
fi
```

\#非空返回true，可使用$?验证（0为true，>1为false）

应用实例

[ Edu ] # 非空，返回true

[ ] # 返回false，中间一定要有空格

[ condition ] && echo OK || echo notok 条件满足，执行后面的语句

常用判断条件

1）=字符串比较

2）两个整数的比较

-lt 小于

-le 小于等于

-eq 等于

-gt 大于

-ge 大于等于

-ne 不等于

3）按照文件权限进行判断

-r 有读的权限

-w 有写的权限

-x 有执行的权限

4）按照文件类型进行判断

-f 文件存在并且是一个常规的文件

-e 文件存在

-d 文件存在并是一个目录

应用实例

案例1：“ok”是否等于“ok”

判断语句：使用 =

案例2：23是否大于等于22

判断语句：使用 -ge

案例3：/root/shcode/aaa.txt目录中的文件是否存在

判断语句：使用 -f

```bash
#!/bin/bash
# 案例1：“ok”是否等于“ok”
# 判断语句：使用 =
if [ "ok" = "ok" ]
then
        echo "equal"
fi
# 案例2：23是否大于等于22
# 判断语句：使用 -ge
if [ 23 -ge 22 ]
then
        echo "大于"
fi
# 案例3：/root/shcode/aaa.txt目录中的文件是否存在
# 判断语句：使用 -f
if [ -f /root/shcode/aaa.txt ]
then
        echo "存在"
fi
```

<br>

## 流程控制

### if判断

```bash
if [ 条件判断式 ]
then
	代码
fi
```

多分支-if判断

```bash
if [ 条件判断式 ]
then
	代码
elif [ 条件判断式 ]
then
	代码
fi
```

注意事项：[ 条件判断式 ]，中括号和条件判断式之间必须有空格

应用实例ifCase.sh

案例：请编写要给shell程序，如果输入的参数，大于等于60，则输出“及格了”，如果小于60，则输出”不及格“

```bash
#!/bin/bash
# 案例：请编写要给shell程序，如果输入的参数，大于等于60，则输出“及格了”，如果小于60，则输出”不及格“
if [ $1 -ge 60 ]
then
        echo "及格了"
elif [ $1 -lt 60 ]
then
        echo "不及格"
fi
```

<br>

### case语句

基本语法

```bash
case $变量名 in
”值1“)
如果变量的值等于值1，则执行程序1
;;
“值2”)
如果变量的值等于值2，则执行程序2
;;
...省略其他分支...
*）
如果变量的值都不是以上的值，则执行此程序
;;
esac # case单词反写
```

应用实例 testCase.sh

案例1：当命令行参数是1时，输出“周一”，是2时，就输出“周二”，其他情况输出“other”

```bash
#!/bin/bash
# 案例1：当命令行参数是1时，输出“周一”，是2时，就输出“周二”，其他情况输出“other”
case $1 in
"1")
echo "周一"
;;
"2")
echo "周二"
;;
*)
echo "other"
esac
```

<br>

### for循环

基本语法1

```bash
for 变量 in 值1 值2 值3... # 值外面可以加双引号
do
	程序/代码
done
```

应用实例testFor1.sh

案例1：打印命令行输入的参数[这里可以看出$*和$@的区别]

```bash
#!/bin/bash
# 案例1：打印命令行输入的参数[这里可以看出$* 和$@ 的区别]
# 注意：$* 是把输入的参数，当作一个整体，所以，只会输出一句话
for i in "$*"
do
        echo "num is $i"
done
# 使用$@ 来获取输入的参数，注意，这时是分别对待，所以有几个参数，就输出几句
echo "==========================="
for i in $@
do
        echo "num is $i"
done
```

基本语法2

```bash
for((初始值;循环控制条件;变量变化))
do
	程序/代码
done
```

应用实例testFor2.sh

案例1：从1加到100的值输出显示

```bash
#!/bin/bash
# 案例1：从1加到100的值输出显示，如何把100做成一个变量
# 定义一个变量
SUM=0
for (( i=1; i<=$1; i++ ))
do
# 写上你的业务代码
        SUM=$[$SUM+$i]
done
echo "总和SUM=$SUM"
```

<br>

### while循环

基本语法1

```bash
while [ 条件判断式 ]
do
程序
done
```

注意：while和[有空格，条件判断式和[也有空格
应用实例testWhile.sh

案例1：从命令行输入一个数n，统计从1+…+n的值是多少？

```bash
#!/bin/bash
# 案例1：从命令行输入一个数n，统计从1+...+n的值是多少？
SUM=0
i=0
while [ $i -le $1 ]
do
        SUM=$[$SUM+$i]
        # i自增
        i=$[$i+1]
done
echo "执行结果=$SUM"
```

<br>

## read读取控制台输入

**基本语法**

read(选项)(参数)

选项：

-p：指定读取值的提示符

-t：指定读取值时等待的时间（秒），如果没有在指定的时间内输入，就不再等待了。

参数：

变量：指定读取值的变量名

**应用实例testRead.sh**

案例1：读取控制台输入一个NUM1值

案例2：读取控制台输入一个NUM2值，在10秒内输入

```bash
#!/bin/bash
# 案例1：读取控制台输入一个NUM1值
read -p "请输入一个数NUM1=" NUM1
echo "你输入的NUM1=$NUM1"
# 案例2：读取控制台输入一个NUM2值，在10秒内输入
read -t 10 -p "请输入一个数NUM2=" NUM2
echo "你输入的NUM2=$NUM2"                        
```

<br>

## 函数

shell编程和其他编程语言一样，有系统函数，也可以自定义函数。

### 系统函数

basename基本语法

功能：返回完整路径最后/的部分，常用于获取文件名

basename [pathname] [suffix]

basename [string] [suffix] (功能描述：basename命令会删掉所有的前缀包括最后一个(‘/’)字符，然后将字符串显示出来。

选项：

suffix为后缀，如果suffix被指定了，basename会将pathname或string中的suffix去掉。

应用实例

案例1：请返回/home/aaa/test.txt的”test.txt“部分

```bash
[root@lmh100 shcode]# basename /home/aaa/test.txt
test.txt
[root@lmh100 shcode]# basename /home/aaa/test.txt .txt
test
```

dirname基本语法

功能：返回完整路径最后/的前面的部分，常用于返回路径部分

dirname 文件绝对路径（功能描述：从给定的包含绝对路径的文件名中去除文件名（非目录的部分），然后返回剩下的路径（目录的部分）

应用实例

案例1：请返回/home/aaa/test.txt的/home/aaa

```bash
[root@lmh100 shcode]# dirname /home/aaa/test.txt
/home/aaa
```

<br>

### 自定义函数

基本语法

```bash
# 函数定义
function funname[()]
{
	Action;
	[return int;]
}
# 调用函数时，直接写函数名
funname [值]
```

应用实例

案例1：计算输入两个参数的和（动态获取），getSum

```bash
#!/bin/bash
# 案例1：计算输入两个参数的和（动态获取），getSum
# 定义函数 getSum
function getSum(){

        SUM=$[$n1+$n2]
        echo "和是=$SUM"

}

# 输入两个值
read -p "请输入一个数n1=" n1
read -p "请输入一个数n2=" n2

# 调用自定义函数
getSum $n1 $n2
```

<br>

##  Shell编程综合案例

### 需求分析

1.每天凌晨2：30备份数据库mzlmh到/data/backup/db

2.备份开始和备份结束能够给出相应的提示信息

3.备份后的文件要求以备份时间为文件名，并打包成.tar.gz的形式，比如：2021-03-12_230201.tar.gz

4.在备份的同时，检查是否有10天前备份的数据库文件，如果有就将其删除。

![image-20230717185434628](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827295.png)

```bash
cd /user/sbin
vim mysql_db_backup.sh

//.sh文件内容
#!/bin/bash
# 备份目录
BACKUP=/data/backup/db
# 当前时间
DATETIME=$(date +%Y-%m-%d_%H%M%S)
echo $DATETIME
# 数据库的地址
HOST=localhost
# 数据库的用户名
DB_USER=root
# 数据库密码
DB_PW=hspedu100
# 备份的数据库名
DATABASE=hspedu

# 创建备份目录，如果不存在，就创建
[ ! -d "${BACKUP}/${DATETIME}" ] && mkdir -p "${BACKUP}/${DATETIME}"

# 备份数据库
mysqldump -u${DB_USER} -p${DB_PW} --host=${HOST} -q -R --databases ${DATABASE} | gzip > ${BACKUP}/${DATETIME}/${DATETIME}.sql.gz

# 将文件处理成 tar.gz(压缩打包）
cd ${BACKUP}
tar -zcvf $DATETIME.tar.gz ${DATETIME}
# 删除对应的备份
rm -rf ${BACKUP}/${DATETIME}

# 删除十天前的备份文件(atime是访问时间)
find ${BACKUP} -atime +10 -name "*.tar.gz" -exec rm -rf {} \;
echo "备份数据库${DATEBASE} 成功~"
//.sh文件内容结束

crontab -e
# 填入内容
30 2 * * * /usr/sbin/mysql_db_backup.s
```

<br>

# 日志管理

1.日志文件是重要的系统信息文件，其中记录了许多`重要的系统事件`，包括用户的登录信息、系统的启动信息、系统的安全信息、邮件的相关信息、各种服务相关信息等。

2.日志对于安全来说也很重要，它记录了系统每天发生的各种事情，通过日志来检查错误发生的原因，或者受到攻击时攻击者留下的痕迹。

3.可以这样理解，日志是用来记录重大事件的工具。

<br>

## 系统常用的日志

/var/log/目录就是系统日志文件的保存位置

![image-20230718182430089](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827296.png)

（下表标红为必须要知道的）

![image-20230718182652837](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827297.png)

注意：这里lastlog拼错了

<br>

## 日志管理服务rsyslogd

CentOS7.6日志服务是rsyslogd，CentOS6.x日志服务是syslogd。rsyslogd功能更强大。

rsyslogd的使用、日志文件的格式，和syslogd服务兼容的。

原理示意图（日志管理服务和日志的关系）：

![image-20230718183702775](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827298.png)

查看Linux中的rsyslogd服务是否启动

```bash
ps aux | grep “rsyslog” | grep -v “grep”
# grep -v “grep”中的-v表示反向进行匹配，这样可以过滤掉查询的本身这条grep指令
```

查询rsyslogd服务的自启动状态

```bash
systemctl list-unit-files | grep "rsyslog"
```

配置文件：`/etc/rsyslog.conf` [重点]

编辑文件时的格式为： \*.\*  存放日志文件

其中第一个\*代表日志类型，第二个\*代表日志级别

1.日志类型分为:

```bash
auth		## pam产生的日志
authpriv	## ssh、ftp等登录信息的验证信息
corn		## 时间任务相关
kern		## 内核
lpr			## 打印
mail		## 邮件
mark(syslog)-rsyslog	## 服务内部的信息，时间标识
news		## 新闻组
user		## 用户程序产生的相关信息
uucp		## unix to unix copy主机之间相关的通信
local 1-7	## 自定义的日志设备
```

2.日志级别分为：

```bash
debug		## 有调用信息的，日志通信最多
info		## 一般信息日志，最常用
notice		## 最具有重要性的普通条件的信息
warning		## 警告级别
err			## 错误级别，阻止某个功能或者模块不能正常工作的信息
crit		## 严重级别，阻止整个系统或者整个软件不能正常工作的信息
alert		## 需要立刻修改的信息
emerg		## 内核崩溃等重要信息
none		## 什么都不记录
```

注意：从上到下，级别从低到高，记录信息越来越少

由日志服务rsyslogd记录的日志文件，日志文件的格式包含以下4列：

1.事件产生的时间

2.产生事件的服务器的主机名

3.产生事件的服务名或程序名

4.事件的具体信息

日志如何查看实例

查看一下/var/log/secure日志，这个日志中记录的是用户验证和授权方面的信息，来分析如何查看

![image-20230718184930184](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827299.png)

日志管理服务应用实例

在/etc/rsyslog.conf中添加一个日志文件/var/log/mzl.log，当有事件发送时（比如sshd服务相关事件），该文件会接收到信息并保存，演示`重启，登录的情况`，看看是否有日志保存
![image-20230718185535289](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827300.png)

<br>

## 日志轮替

日志轮替就是把旧的日志文件移动并改名，同时建立新的空日志文件，当旧日志文件超出保存的范围之后，就会进行删除

### 日志轮替文件命名

1.centos7使用logrotate进行日志轮替管理，要想改变日志轮替文件名字，通过/etc/logrotate.conf配置文件中"dateext"参数；

2.如果配置文件中有“dateext”参数，那么日志会用<font color="red">日期</font>来作为日志文件的后缀，例如“secure-20201010”。这样日志文件名不会重叠，也就不需要日志文件的改名，只需要指定保存日志个数，删除多余的日志文件即可。

3.如果配置文件中没有“dateext”参数，日志文件就需要进行改名了。当第一次进行日志轮替时，<font color="red">当前的“secure”日志会自动改名为“secure.1”，然后新建”secure“日志，用来保存新的日志</font>。当第二次进行日志轮替时，”secure.1“会自动改名为”secure.2“，当前的”secure“日志会自动改名为”secure.1“，然后也会新建”secure“日志，用来保存新的日志，以此类推。

注意：

1./etc/logrotate.conf里既可以配置全局的日志轮替策略/规则，也可以单独给某个日志文件指定策略。

2.也可以把某个日志文件的轮替规则，写到/etc/logrotate.d目录，

<br>

### logrotate配置文件

/etc/logrotate.conf为logrotate的全局配置文件

![image-20230718190105442](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827301.png)

参数说明

```bash
参数					参数说明
daily				日志的轮替周期是每天
weekly				日志的轮替周期是每周
monthly				日志的轮替周期是每月
rotate 数字			保留的日志备份文件的个数。0指没有备份。
compress			日志轮替时，旧的日志进行压缩
create mode owner group 建立新日志，同时指定新日志的权限与所有者和所属组
mail address		当日志轮替时，输出内容通过邮件发送到指定的邮件地址
missingok			如果日志不存在，则忽略该日志的警告信息
notifempty			如果日志为空文件，则不进行日志轮替
minsize大小			日志轮替的最小值。也就是日志一定要达到这个最小值才会轮替，否则就算时间达到也不轮替
size大小				日志只有大于指定大小才进行日志轮替，而不是按照时间轮替
dateext				使用日期作为日志轮替文件的后缀
sharedscripts		在此关键字后的脚本只执行一次
prerotate/endscript	在日志轮替之前执行脚本命令
postrotate/endscript在日志轮替之后执行脚本命令
```

<br>

### 把自己的日志加入日志轮替

第一种方法是直接在/etc/logrotate.conf配置文件中写入该日志的轮替策略。

第二种方法是在/etc/logrotate.d/目录中新建立该日志的轮替文件，在该轮替文件中写入正确的轮替策略，因为该目录中的文件都会被”include“到主配置文件中，所以也可以把日志加入轮替。

推荐使用第二种方法，因为系统中需要轮替的日志非常多，如果全都直接写入/etc/logrotate.conf配置文件，那么这个文件的可管理性就会非常差，不利于此文件的维护。

在/etc/logrotate.d/配置轮替文件一览

**应用实例**

在/etc/logrotate.conf进行配置，或者直接在/etc/logrotate.d/下创建mzllog编写如下内容，具体轮替的效果可以参考/var/log下的boot.log情况

```bash
/var/log/mzl.log
{
	missingok
	daily
	copytruncate # 拷贝截断：用于还在打开中的日志文件，将当前日志备份重命名并将原文件清空
	rotate 7
	notifempty
}
```

<br>

## 日志轮替机制原理

日志轮替之所以可以在指定的时间备份日志，是依赖于系统定时任务。

在/etc/cron.daily/目录，就会发现这个目录中是由logrotate文件（可执行），logrotate通过这个文件依赖定时任务执行的。

![image-20230718194530049](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827302.png)

<br>

## 查看内存日志

有一部分日志先写到内存里面，还没有写到文件里。

journalctl 可以查看内存日志，这里我们看看常用的指令

```bash
journalctl ## 查看全部
journalctl -n 3 ## 查看最新3条
journalctl --since 19:00 --until 19:10:10 # 查看起始时间到结束时间的日志可加日期
journalctl -p err ## 报错日志
journalctl -o verbose ## 日志详细内容
journalctl _PID=1245 _COMM=sshd	##查看包含这些参数的日志（在详细日志查看）
或者journalctl | grep sshd
```

注意：journalctl 查看得是`内存日志`，重启会清空

<br>

# 定制自己的Linux系统

通过裁剪现有Linux系统（CentOS7.6），创建属于自己的min Linux小系统，可以加深我们对linux的理解。

## 基本原理

启动流程介绍：

制作Linux小系统前，再了解一下Linux的启动流程：

1.首先Linux要通过自检，检查硬件设备有没有故障

2.如果有多块启动盘的话，需要在BIOS中选择启动磁盘

3.启动MBR中的bootloader引导程序

4.加载内核文件

5.执行所有进程的父进程、老祖宗systemd

6.欢迎界面

在Linux的启动流程中，加载内核文件时关键文件:

1）kernel文件：vmlinuz-3.10.0-957.el7.x86_64

2）initrd文件：initramfs-3.10.0-957.el7.x86_64.img

<br>

## 制作min linux思路分析

1.在现有的Linux系统（centos7.6）上加一块硬盘/dev/sdb，在硬盘上分两个分区，一个是/boot，一个是/，并将其格式化。需要明确的是，现在加的这个硬盘在现有的Linux系统中是/dev/sdb，但是，当我们把东西全部设置好时，要把这个硬盘拔除，放在新的系统上，此时，就是/dev/sda。

2.在/dev/sdb硬盘上，将其打造成独立的Linux系统，里面的所有文件是需要拷贝进去的

3.作为能独立运行的Linux系统，内核是一定不能少，要把内核文件和initramfs文件也一起拷到/dev/sdb上

4.以上步骤完成，我们的自制Linux就完成，创建一个新的linux虚拟机，将其硬盘指向我们创建的硬盘，启动即可

<br>

## 操作步骤

1.首先，我们在现有的linux添加一块大小为20G的硬盘（注意添加过程中，一定要将虚拟磁盘存储为单个文件）

注意：由于之前在学习挂载的时候，添加了一块硬盘，所以移除后如果不在配置文件/etc/fstab将永久挂载点删除，则开机会进入紧急模式。解决的办法是：进入紧急模式后，输入root密码，编辑/etc/fstab，将之前的sdb硬盘挂载指示内容删除，再重启。如果开机进入了紧急模式，可能是因为刚刚移除硬盘后

2.接下来进行分区和格式化

1）先用lsblk查看目前有哪些硬盘。

2）对硬盘进行分区

```bash
fdisk /dev/sdb
```

3）对硬盘进行格式化

```bash
mkfs.ext4 /dev/sdb1
mkfs.ext4 /dev/sdb2
```

4）创建目录，并挂载新的磁盘

```bash
mkdir -p /mnt/boot /mnt/sysroot # 创建目录
mount /dev/sdb1 /mnt/boot/
mount /dev/sdb2 /mnt/sysroot/
```

5）安装grub2，内核文件拷贝至目标磁盘

```bash
grub2-install --root-directory=/mnt /dev/sdb # 安装grub2
hexdump -C -n 512 /dev/sdb # 验证是否安装成功
rm -rf /mnt/boot/* # 清除/mnt/boot/下原有的内容
cp -rf /boot/* /mnt/boot/ # 将sda的boot/中的所有内容拷贝到/mnt的boot/下（相当于到sdb1）
```

6）修改grub2/grub.cfg文件中的UUID（指定那些盘是启动盘，哪些盘是根目录盘），标红的部分是需要使用指令来查看的

（用sed -i全部替换更方便）

在UTF-8后面要加一句话selinux=0 init=/bin/bash，代表不要走系统那条线，要走我自己定制的shell

在linux16最后也要加上selinux=0 init=/bin/bash

保存退出

7）创建目标主机根文件系统（将所有重要目录建起来，虽然是空的）(注意/mnt/sysroot和{}之间没有空格)

```bash
mkdir -pv /mnt/sysroot/{etc/rc.d,usr,var,proc,sys,dev,lib,lib64,bin,sbin,boot,srv,mnt,media,home,root}
```

8）拷贝需要的bash(也可以拷贝你需要的指令）和库文件给新的系统使用

```bash
cp /lib64/*.* /mnt/sysroot/lib64/
cp /bin/bash /mnt/sysroot/bin/
```

9.原虚拟机先关机，然后新创建一个虚拟机，然后将默认分配的硬盘移除掉，指向我们刚刚创建的磁盘即可。

10）这时，很多指令都不能使用，比如ls，reboot等，可以将需要的指令拷贝到对应的目录即可

11)如果要拷贝指令，重新进入到原来的linux系统拷贝相应的指令即可，如将/bin/ls拷贝到/mnt/sysroot/bin，将/sbin/reboot拷贝到/mnt/sysroot/sbin

```bash
mount /dev/sdb2 /mnt/sysroot/ # 重新挂载
cp /bin/ls /mnt/sysroot/bin/
cp /bin/systemctl /mnt/sysroot/bin/
cp /sbin/reboot /mnt/sysroot/sbin/
```

12）再重启新的min linux系统，就可以使用ls，reboot指令了

<br>

# 额外阅读：Linux内核源码-介绍&内核升级

linux0.01内核源码

基本介绍

Linux的内核源代码可以从网上下载，解压缩后文件文件一般也都位于linux目录下。内核源代码有很多版本，可以从linux0.01内核入手，总共的代码1w行左右，版本5.9.8总共代码超过700w行，非常庞大。

内核地址：https://www.kernel.org/

linux0.01内核源码目录&阅读

阅读内核源码技巧

1.linux0.01的阅读需要懂c语言

2.阅读源码前，应知道Linux内核源码的整体分布情况。现代的操作系统一般由进程管理、内存管理、文件系统、驱动程序和网络等组成。Linux内核源码的各个目录大致与此相对应。

3.在阅读方法或顺序上，有纵向与横向之分。所谓纵向就是顺着程序的执行顺序逐步进行（比如从主方法开始阅读）；所谓横向，就是按模块进行，它们常结合在一起进行（比如先看内存管理mm模块）。

4.对于Linux启动的代码可顺着Linux的启动顺序一步步来阅读；对于像内存管理部分，可以单独拿出来进行阅读分析。实际上这是一个反复的过程，不可能读一遍就理解

linux内核源码阅读&目录介绍&main.c说明

![image-20230719154218014](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827303.png)

main.c中

![image-20230719154646277](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827304.png)

linux内核升级应用实例

将Centos系统从7.6内核升级到7.8版本内核（兼容性问题）

具体步骤

```bash
uname -a //查看当前的内核版本
yum info kernel -q //检测内核版本，显示可以升级的内核
yum update kernel //升级内核
yum list kernel -q //查看已经安装的内核
```

注意：装了新的内核后，使用uname -a，仍显示原内核。重启之后，在重启界面可以选择新内核。新内核是兼容原先的系统的。

 <br>

# 备份与恢复

实体机无法做快照，如果系统出现异常或者数据损坏，后果严重，要重做系统，还会造成数据丢失。所以我们可以使用备份和恢复技术

linux的备份和恢复很简单，有两种方式：

1.把需要的文件（或者分区）用TAR打包就行，下次需要恢复的时候，再解压开覆盖即可

2.使用dump和restore命令

## 安装dump和restore

如果linux上没有dump和restore指令，需要先安装

```bash
yum -y install dump (可能会同时安装上restore）
yum -y install restore
```

<br>

## 使用dump完成备份

dump支持分卷和增量备份（所谓增量备份是指备份上次备份后 修改/增加过的文件，也称差异备份）。

示意图:

第一次备份层级为0，表示完整备份。后面的层级表示增量备份/差异备份/层级备份。
![在这里插入图片描述](https://img-blog.csdnimg.cn/3b59dad1b58d41c6a47556365d95c55c.png)

<br>

### dump语法说明

dump [-cu] [-123456789] [-f <备份后文件名>] [-T <日期>] [目录或文件系统]

dump []-wW

-c（c是一个数字，可以是0-9中的一个数字）：创建新的归档文件，并将由一个或多个文件参数所指定的内容写入归档文件的开头。

-0123456789：备份的层级。0为最完整备份，会备份所有文件。若指定0以上的层级，则备份自上一次备份以来修改或新增的文件，到9后，可以再次轮替。

-f <备份后文件名>：指定备份后文件名

-j：`调用bzlib库压缩备份文件`，也就是将备份后的文件压缩成bz2格式，让文件更小

-T <日期>：指定开始备份的时间与日期

-u：备份完成后，在/etc/dumpdares中记录备份的文件系统，层级，日期与时间等。（不带u则不知道备份到第几次了）

-t：指定文件名，若该文件已存在备份文件中，则列出名称

-W：显示需要备份的文件及其最后一次备份的层级，时间，日期。

-w：与-W类似，但仅显示需要备份的文件。

**应用案例1**

将/boot分区所有内容备份到/opt/boot.bak.bz2文件中，备份层级为’0’

```bash
dump -0uj -f /opt/boot.bak0.bz2 /boot
```

**dump应用案例2**

在/boot目录下增加新文件，备份层级为"1"（只备份上次使用层级”0“备份后发生过改变的数据），`注意比较看看这次生成的boot1.bak有多大`

```bash
dump -1uj -f /opt/boot.bak1.bz2 /boot
```

![image-20230719162725892](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827305.png)

注意：通过dump命令在配合cronbtab可以实现无人值守备份

<br>

### dump -W

显示需要备份的文件及其最后一次备份的层级，时间，日期
![image-20230719162813992](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827306.png)

<br>

### 查看备份时间文件

```bash
cat /etc/dumpdates
```

![image-20230719162905785](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827307.png)

<br>

### dump备份文件或者目录

前面我们在备份分区时，是可以支持增量备份的，如果备份`文件或者目录，不再支持增量备份`，即只能只用0级别备份

案例：使用dump备份/etc整个目录

dump -0j -f /opt/etc0.bak.bz2 /etc/

```bash
# 下面这条语句会报错，提示DUMP：Only level0 dumps are allowed on a subdirectory
dump -1j -f /opt/etc.bak.bz2 /etc/
```

注意：重要的备份文件，比如数据区，建议将文件上传到其他服务器保存，不要将鸡蛋放在同一个篮子。

<br>

## 使用restore完成恢复

restore命令用来恢复已备份的文件，可以从dump生成的备份文件中恢复原文件

### restore基本语法

restore [模式选项] [选项]

说明下面四个模式，不能混用，在一次命令中，只能指定一种。

-C：使用对比模式，将备份的文件与已存在的文件相互对比。

-i：使用交互模式，在进行还原操作时，restores指令将依序询问用户

-r：进行还原模式（用的最多的模式）

-t：查看模式，看备份文件有哪些文件

选项

-f <备份设备>：从指定的文件中读取备份数据，进行还原操作

**应用案例1**

restore命令比较模式，比较备份文件和原文件的区别

测试

```bash
mv /boot/hello.java /boot/hello100.java

restore -C -f boot.bak1.bz2 //注意和最新的文件比较
```

![image-20230719163513000](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827308.png)

mv /boot/hello100.java /boot/hello.java

restore -C -f boot.bak1.bz2

![image-20230719163618351](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827309.png)

**应用案例2**

restore命令查看模式，看备份文件中有哪些数据/文件

测试

```bash
restore -t -f boot.bak1.bz2
```

![image-20230719163756410](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307191827310.png)

**应用案例3**

restore命令还原模式，`注意细节`：如果你有增量备份，需要把增量备份文件也进行恢复，有几个增量备份文件，就要恢复几个，按顺序来恢复即可。

测试

```bash
mkdir /opt/boottmp
cd /opt/boottmp
restore -r -f /opt/boot.bak0.bz2 //恢复到第一次完全备份状态
restore -r -f /opt/boot.bak1.bz2 //恢复到第二次增量备份状态
```

**应用案例4**

restore命令恢复备份的文件，或者整个目录的文件

基本语法：restore -r -f 备份好的文件

测试

```bash
mkdir etctmp
cd etctmp/
restore -r -f /opt/etc.bak0.bz2
```