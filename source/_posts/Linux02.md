---
title: Linux学习02
date: 2023-07-16 19:37:55
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307161939739.jpg
tags:
  - Linux
  - CentOS
updated:
categories: 学习笔记
description: Linux基础知识，组管理，权限管理，定时任务调度，磁盘，网络配置，进程管理，rpm，yum
mathjax:
toc: true
toc_number: true
---

# 组管理和权限管理

在linux中的每个用户必须属于一个组，不能独立于组外。在linux中每个文件有所有者、所在组、其他组的概念。

1.所有者（谁创建谁就是所有者，但是后续可以修改）

2.所在组（创建者所属用户组，但后续可以修改）

3.其他组（除文件的所有者和所在组的用户外，系统的其他用户都是文件的其他组）

4.改变用户所在的组

<br>

## 文件/目录 所有者

一般为文件的创建者，谁创建了该文件，就自然地成为该文件的所有者。

**查看文件的所有者**

指令：ls -ahl

**修改文件所有者（ch-ange own-er）**

指令：chown 用户名 文件名

要求：使用root创建一个文件apple.txt，然后将其所有者修改成tom：

```bash
touch apple.txt
chown tom apple.txt # tom用户必须存在
```

注意：改变所有者，所在组不会变

<br>

## 组的创建

基本指令：groupadd 组名

**应用实例**

创建一个组，monster：groupadd monster

创建一个用户fox，并放入monster组中：useradd -g monster fox

<br>

## 文件/目录 所在组

当某个用户创建了一个文件后，这个文件的所在组就是该用户所在的组。(默认）

**查看文件/目录所在组**

基本指令：ls -ahl

应用实例

使用fox创建一个文件，看看该文件属于哪个组？

-rw-r–r–. 1 fox monster 0 1月 7 12:50 ok.txt

**修改文件/目录所在的组**

基本指令：chgrp 组名 文件名

应用实例

使用root用户创建文件orange.txt，看看当前这个文件属于哪个组，然后将这个文件所在组，修改到fruit组。

```shell
groupadd fruit
touch orange.txt
看看当前这个文件属于哪个组 -> root组
chgrp fruit orange.txt
```

<br>

## 改变用户所在组

在添加用户时，可以指定将该用户添加到哪个组中，同样的，用root的管理权限可以改变某个用户所在的组。

**改变用户所在组**

1.usermod -g 新组名 用户名

2.usermod -d 目录名 用户名 改变该用户登录的初始目录（特别说明：用户需要有进入到新目录的权限）

应用实例

将zwj这个用户从原来所在组，修改到wudang组：usermod -g wudang zwj

<br>

## 权限的基本介绍

ls -l中显示的内容如下：

![image-20230713102605434](D:\typora图片\image-20230713102605434.png)

第一列一共有10位，分别用0-9表示，下面是0-9位的说明：

1.第0位确定文件类型（d，-，l，c，b）

l是链接，相当于windows的快捷方式

d是目录，相当于windows的文件夹

-是普通文件

c是字符设备文件，鼠标，键盘（cd /dev可以看到）

b是块设备，比如硬盘（cd /dev可以看到）

2.第1-3位确定所有者（该文件的所有者）拥有该文件的权限。 —User

3.第4-6位确定所在组（同用户组的）拥有该文件的权限。—Group

4.第7-9位确定其他用户拥有该文件的权限。—Other

<br>

## rwx权限详解(难点）

**rwx作用到文件**

1.[r]代表可读（read）：可以读取，查看

2.[w]代表可写（write）：可以修改，但是不代表可以删除该文件，删除一个文件的前提条件是对该文件所在的目录有写权限，才能删除该文件

3.[x]代表可执行（execute）：可以被执行

**rwx作用到目录**

1.[r]代表可读（read）：可以读取，ls查看目录内容

2.[w]代表可写（write）：可以修改，对目录内创建+删除+重命名目录

3.[x]代表可执行（execute）：可以进入该目录

<br>

## 文件及目录权限实际案例

=ls -l中显示的内容如下：

-rwxrw-r-- 1 root root 1213 Feb 2 09:39 abc

- 10个字符确定不同用户能对文件干什么

  第一个字符代表文件类型：-代表该文件是普通文件

  其余字符每3个一组（rwx）读（r）写（w）执行（x）

  第一组rwx：文件拥有者的权限是读、写和执行

  第二组rw-：所在组的用户的权限是读、写但不能执行

  第三组r–： 其他组的用户的权限是读不能写和执行

- 可用数字表示：r=4，w=2，x=1，因此rwx=4+2+1=7，数字可以进行组合

- 其他说明

  1 文件：该数一定为1 目录：该数对应子目录数

  root 所有者

  root 所在组

  1213 大小（字节），如果是文件夹，显示4096字节

  Feb 2 09:39 最后修改日期

  abc 文件名

<br>

## 修改权限-chmod

**第一种方式：+、-、=变更权限**

u:所有者 g：所有组 o：其他人 a：所有人（u、g、o的总和）

1.chmod u=rwx,g=rx,o=x 文件/目录名

2.chmod o+w 文件/目录名

3.chmod a-x 文件/目录名

案例演示

1.给abc文件的所有者读写执行的权限，给所在组读执行权限，给其他组读执行权限：chmod u=rwx, g=rx,o=rx abc

2.给abc文件的所有者除去执行的权限，增加组写的权限:chmod u-x g+w abc

3.给abc文件的所有用户添加读的权限:chmod a+r abc

注意：当文件可执行的时候，它的颜色会变成绿色

**第二种方式：通过数字变更权限**

r=4 w=2 x=1 rwx=4+2+1=7

chmod u=rwx,g=rx,o=x 文件目录名 相当于chmod 751 文件目录名

案例演示

要求：将/home/abc.txt 文件的权限修改成rwxr-xr-x，使用给数字的方式实现：

chmod 755 /home/abc.txt

<br>

## 修改文件所有者-chown

chown newowner 文件/目录 (功能描述：改变所有者）

chown newowner:newgroup 文件/目录 （功能描述：改变所有者和所在组）
-R 如果是目录，则使其下所有子文件或目录递归生效

案例演示

请将/home/abc.txt文件的所有者修改成tom：chown tom /home/abc.txt

请将/home/test目录下所有的文件和目录的所有者都修改成tom：chown -R tom /home/test

<br>

## 修改文件/目录所在组-chgrp

基本介绍

chgrp newgroup 文件/目录

案例演示

请将/home/abc.txt文件的所在组修改成shaolin(少林)：chgrp shaolin /home/abc.txt

请将/home/test目录下所有的文件和目录的所在组都修改成shaolin（少林）：chgrp -R shaolin /home/test

<br>

# 定时任务调度

## Crond任务调度

任务调度：是指系统在某个时间执行的特定的**命令**或**程序**。

任务调度分类：

1.系统工作：有些重要的工作必须周而复始地执行。如病毒扫描等

2.个别用户工作：个别用户可能希望执行某些程序，比如对mysql数据库的备份

**基本语法：** crontab [选项]

| 选项 | 作用                          |
| ---- | ----------------------------- |
| -e   | 编辑crontab定时任务           |
| -l   | 查询crontab任务               |
| -r   | 删除当前用户所有的crontab任务 |

注意

`-r`  是删除当前用户所有任务，如果只想删除某个任务，使用 `-e ` 进入编辑，删除那行任务或者 `#` 注释那行任务

案例

设置任务调度文件：/etc/crontab

设置个人任务调度。执行crontab -e命令

接着输入任务到调度文件

如：*/1 * * * * ls -l /etc/ > /tmp/to.txt（注意：*之间有空格）

意思说每小时的每分钟执行ls -l /etc/ > /tmp/to.txt命令

![image-20230714145621483](D:\typora图片\image-20230714145621483.png)

![image-20230714145659346](D:\typora图片\image-20230714145659346.png)

![image-20230714145815433](D:\typora图片\image-20230714145815433.png)

<br>

## at定时任务

1.at命令是一次性定时计划任务，at的守护进程atd会以后台模式运行，检查作业队列来运行。

2.默认情况下，atd守护进程每60秒检查作业队列，有作业时，会检查作业运行时间，如果时间与当前时间匹配，则运行此作业。

3.at命令是一次性定时计划任务，执行完一个任务后不再执行此任务了

4.在使用at命令的时候，一定要保证atd进程的启动，可以使用相关指令来查看

指令ps -ef（检测当前所有正在运行的进程有哪些）

指令ps -ef | grep atd （过滤其中的atd相关指令)//可以检测atd是否在运行

**at命令格式：**at [选项] [时间]

Crtl + D 结束at命令的输入（按两次)

![image-20230714152821048](D:\typora图片\image-20230714152821048.png)

**at时间定义**

at指定时间的方法:

1.接受在当天的hh:mm(小时：分钟）式的时间指定。假如该时间已过去，那么就放在第二天执行。例如：04：00

2.使用midnight（深夜），noon（中午），teatime（饮茶时间，一般是下午4点）等比较模糊的词语来指定时间。

3.采用12小时计时制，即在时间后面加上AM（上午）或PM（下午）来说明是上午还是下午。例如：12pm

4.指定命令执行的具体日期，指定格式为month day（月 日）或mm/dd/yy（月/日/年）或dd.mm.yy（日.月.年），指定的日期必须跟在指定时间的后面。例如：04:00 2021-03-1

5.使用相对计时法。指定格式为：now + count time-units，now就是当前时间，time-units是时间单位，这里能够使minutes（分钟）、hours（小时）、days（天）、weeks（星期）。count是时间的数量，几天，几小时。例如：now + 5 minutes

6.直接使用today（今天）、tomorrow（明天）来指定完成命令的时间。

**应用案例**

案例1：2天后的下午5点执行/bin/ls /home

```shell
at 5pm + 2days
at> /bin/ls /home # 然后按两次ctrl+D
```

案例2：atq命令来查看系统中没有执行的工作任务：atq

案例3：明天17点钟，输出时间到指定文件内，比如/root/date100.log

```shell
at 5pm tomorrow
at> date > /root/date100.log # 然后按两次ctrl+D
```

案例4：2分钟后，输出时间到指定文件内，比如/root/date200.log

```shell
at now + 2 minutes
at> date > /root/date200.log # 然后按两次ctrl+D
```

案例5：删除已经设置的任务，atrm 编号（也可以使用at -d 任务编号：来删除设置的任务）

注意：当出现at>，要输入指令时，退格会失效，要退格，需要使用ctrl+退格

<br>

# 磁盘分区、挂载

## Linux分区

**原理介绍**

1.Linux来说无论有几个分区，分给哪一目录使用，它归根结底就只有一个根目录，一个独立且唯一的文件结构，Linux中每个分区都是用来组成整个文件系统的一部分。

2.Linux采用了一种叫“载入”的处理方法，它的整个文件系统中包含了一整套的文件和目录，且将一个分区和一个目录联系起来。这时要载入的一个分区将使它的存储空间在一个目录下获得。

3.示意图
![image-20230714174037452](D:\typora图片\image-20230714174037452.png)

**硬盘说明**

1.Linux硬盘分IDE硬盘和SCSI硬盘，目前基本上是SCSI硬盘

2.对于IDE硬盘，驱动器标识符为‘hdx~’，其中’hd‘表明分区所在设备的类型，这里是指IDE硬盘。’x‘为盘号（a为基本盘，b为基本从属盘，c为辅助主盘，d为辅助从属盘），’ ~'代表分区，前四个分区用数字1-4表示，它们是主分区或扩展分区，从5开始就是逻辑分区。例如，hda3表示为前一个IDE硬盘上的第三个主分区或扩展分区，hdb2表示为第二个IDE硬盘上的第二个主分区或扩展分区。

3.对于SCSI硬盘则标识为’sdx~‘，SCSI硬盘使用’sd‘来表示分区所在设备的类型的，其余则和IDE硬盘的表示方法一样。

![image-20230714174326670](D:\typora图片\image-20230714174326670.png)

上图中，sda表示第一块SCSI硬盘，sda1表示第一块SCSI硬盘的第一个分区

sr0是光驱

**查看所有设备挂载情况**

命令：lsblk或者lsblk -f （list block的简称）

lsblk -f可以将分区的情况看的更加清晰

![image-20230714174448867](D:\typora图片\image-20230714174448867.png)

FSTYPE是文件类型，UUID是当你格式化后，会给你的每一个分区分配一个唯一的不重复的40位的字符串（唯一标识符)，MOUNTPOINT是挂载点

<br>

## 挂载的经典案例

**如何增加一块硬盘**

1.虚拟机增加硬盘

2.分区

3.格式化

4.挂载

5.设置可以自动挂载

- 虚拟机增加硬盘步骤1
  在[虚拟机]菜单中，选择[设置]，然后设备列表里添加硬盘，然后一路[下一步]，中间只有选择磁盘大小的地方需要修改，直到完成。然后重启系统（才能识别）！

- 虚拟机增加硬盘步骤2
  分区命令 fdisk /dev/sdb
  开始对/sdb分区
  m 显示命令列表
  p 显示磁盘分区 同 fdisk -l
  n 新增分区
  d 删除分区
  w 写入并退出
  说明：开始分区后输入n，新增分区，然后选择p，分区类型为主分区。两次回车默认剩余全部空间。最后输入w写入分区并退出，若不保存退出输入q。

- 虚拟机增加硬盘步骤3
  格式化磁盘
  分区命令：mkfs [选项] [-t 分区格式] 分区路径，例如mkfs -t ext4 /dev/sdb1 （mkfs是make filesystem的简写）（也可以写成mkfs.ext4 /dev/sdb1)
  其中ext4是分区类型

- 虚拟机增加磁盘步骤4
  挂载：将一个分区与一个目录关联起来
  mount 设备名称 挂载目录
  例如：mount /dev/sdb1 /newdisk（必须先创建/newdisk目录）
  卸载：
  umount 设备名称 或者 挂载目录
  例如：umount /dev/sdb1 或者 umount /newdisk
  **注意**：用命令行挂载重启后会失效（即这样操作的挂载关系是临时的）

- 虚拟机增加硬盘步骤5
  永久挂载：通过修改/etc/fstab实现自动挂载（fstab是filesystem table的缩写）

  补充：最后的两个数字：第一个是挂载点内备份，0表示不做dump备份，第二个表示磁盘检查，0表示不检查磁盘扇区，1表示其他目录文件检查，2表示根目录文件检查
  添加完成后 执行mount -a即刻生效（或者执行reboot也会自动生效）
  注意：永久挂载前最好保存快照。防止操作失误导致开不了机

<br>

## 磁盘情况查询

**查询系统整体磁盘使用情况**

基本语法：df -h

应用实例

查询系统整体磁盘使用情况
![image-20230715151325928](D:\typora图片\image-20230715151325928.png)

注意：如果磁盘使用率达到80%以上，就需要想办法清理空间了。

**查看指定目录的磁盘占用情况**

基本语法：du -h /目录

查询指定目录的磁盘占用情况，不指定则默认查询当前目录

-s 指定目录占用大小汇总

-h 带计量单位（人类可读）

-a 含文件

–max-depth=1 子目录深度（当前目录的1层子目录也显示）

-c 列出明细的同时，增加汇总值

应用案例

查询/opt目录的磁盘占用情况，深度为1：du -h --max-depth=1

<br>

## 磁盘情况-工作使用指令

1.统计/opt文件夹下文件（以-开头）的个数

第一步：列出/opt目录下的内容：ls -l /opt

第二步：通过正则表达式‘^-’过滤（因为-开头的是文件类型）：ls -l /opt | grep ‘^-’

第三步：利用wc指令和-l选项统计行数

```shell
ls -l /opt | grep '^-' | wc -l
```

补充：

wc指令(wc就是wordcount的简写)选项：

-l：计算行数（line）

-c：计算字节数（character）

-m：计算字符数（char）

-w：计算单词数（word）

2.统计/opt文件夹下目录的个数

```shell
ls -l /opt | grep '^d' | wc -l
```

3.统计/opt文件夹下文件的个数，包括子文件夹里的

```shell
ls -lR /opt | grep '^-' | wc -l # 递归查看目录
```

4.统计/opt文件夹下目录的个数，包括子文件夹里的

```shell
ls -lR /opt | grep '^d' | wc -l # 递归查看目录
```

5.以树状显示目录结构：tree 目录

注意：如果没有tree指令，则使用yum install tree，前提网络是畅通的

<br>

# 网络配置

## 网络配置原理图

![image-20230715185001874](D:\typora图片\image-20230715185001874.png)

<br>

## linux网络环境配置

**第一种方法（自动获取）：**

说明：登录后，通过界面来设置自动获取ip。

优点：linux启动后会自动获取IP，避免冲突

缺点：每次自动获取的ip地址可能不一样


**第二种方法（指定ip）：**

- 说明
  直接修改配置文件来指定ip，并可以连接到外网（程序员推荐）
- 编辑： `vim /etc/sysconfig/network-scripts/ifcfg-ens33`（ens33就是网络设备，更准确地说是网卡）
  要求：将ip地址配置成静态的：比如：ip地址为192.168.200.130
  ![image-20230715185907656](D:\typora图片\image-20230715185907656.png)
- ifcfg-ens33 文件说明

```vim
TYPE=‘Ethernet’                     # 网络类型（通常是Ethenet）
BOOTPROTO=‘dhcp’				    # IP的配置方法[none|static|bootp|dhcp]（分别表示引导时不使用协议|静态分配IP|BOOTP协议|DHCP协议）
UUID=926a57ba。。。。。              	# 随机id
DEVICE=‘eth0’						# 接口名（设备，网卡）
HWADDR=‘00:0C:2x:0x:xx’	           	# MAC地址（这里没有)
ONBOOT=‘yes’						# 系统启动时，网络接口是否有效（yes/no）
```

要将ip地址配置成静态的，需要修改ifcfg-ens33文件，具体地

```vim
需要修改BOOTPROTO=‘static’		   # 表示静态分配IP
需要添加IPADDR=192.168.200.130 	   # 表示要固定的IP地址
需要添加GATEWAY=192.168.200.2 	   # 表示网关
需要添加DNS1=192.168.200.2		   # 表示域名解析器
```

修改后保存，接下来还需要打开虚拟机的虚拟网络编辑器，找到VMnet8，修改其子网IP为192.168.200.0

接下来打开NAT设置，修改网关IP为192.168.200.2

点应用和确定

- 重启网络服务或者重启系统生效
  service network restart # 重启网络服务
  reboot # 重启系统

注意：因为修改了虚拟机的ip，所有要在xshell中登录虚拟机，需要修改对应连接的属性中的ip

<br>

## 设置主机名和hosts映射

**设置主机名**

1.为了方便记忆，可以给linux系统设置主机名，也可以根据需要修改主机名

2.指令hostname：查看主机名

3.修改文件在/etc/hostname指定

4.修改后，重启生效



**设置hosts映射**

思考：如何通过主机名能够找到（比如ping）某个linux系统?

- windows
  在C:\Windows\System32\drivers\etc\hosts文件指定ip地址和主机名的关系即可
  案例：192.168.200.130 lmh100
- linux
  在/etc/hosts 文件指定
  案例：192.168.200.1 ThinkPad-PC（这个名字其实可以随意，但是ping的时候必须也是这个名称）

<br>

## 主机名解析过程分析（Hosts、DNS）

**Hosts**是一个文本文件，用来记录IP和Hostname（主机名）的映射关系

**DNS**

1.DNS，就是Domain Name System的缩写，翻译过来就是域名系统

2.是互联网上作为域名和IP地址相互映射的一个分布式数据库

应用实例：

用户在浏览器输入了www.baidu.com

1.浏览器先检查浏览器缓存中有没有该域名和IP地址的对应关系，有就先调用这个IP完成解析；如果没有找到，就检查操作系统DNS解析器缓存，如果有直接返回IP完成解析。这两个缓存，可以理解为**本地解析器缓存**。

2.一般来说，当电脑第一次成功访问某一网站后，在一定时间内，浏览器或操作系统会缓存他的IP地址（DNS解析记录)。如在cmd窗口中输入

ipconfig /displaydns // 可以看到当前操作系统里面的DNS域名解析缓存

ipconfig /flushdns // 手动清理dns缓存

3.如果本地解析器缓存没有找到对应映射，检查系统中的hosts文件中有没有配置对应的域名IP映射，如果有，则完成解析并返回。

4.如果本地DNS解析器缓存和hosts文件中均没有找到对应的IP，则到域名服务DNS（公网的DNS，即分布式数据库）进行解析域

5.如果还没找到，返回域名不存在的信息

注意：
1.如果是ping 一个域名，则没有浏览器缓存

2.公网的DNS服务器不是一个服务器，而是分级的（这是为了优化而做的）

示意图

![image-20230715191804156](D:\typora图片\image-20230715191804156.png)

补充：

dns域名劫持：黑客攻击，修改hosts文件，会让域名定向到假网站，（不过现在很多浏览器都有防域名劫持的机制)。

<br>

# 进程管理（重点）

1.在Linux中，每个`执行的程序`都成为一个进程。每一个进程都分配一个ID号(pid，也叫进程号）。
补充：程序（静态概念）不运行的时候，就是一段代码。当它加载到内存里时，它才是一个进程（动态概念)。

2.每个进程都可能以两种方式存在。`前台`与`后台`，所谓前台进程就是用户目前的屏幕上可以进行操作的。后台进程则是实际在操作，但由于屏幕上无法看到的进程，通常使用后台方式执行，比如mysql服务。

3.一般系统的服务都是以后台进程的方式存在，如mysql，tomcat，而且都会常驻在系统中。直到关机才结束。

<br>

## 显示系统执行的进程

ps命令是用来查看目前系统中，有哪些正在执行，以及它们执行的状况。可以不加任何参数。

![image-20230716170005169](D:\typora图片\image-20230716170005169.png)

一般将-a，-u和-x三个参数同时组合使用。

![image-20230716165907670](D:\typora图片\image-20230716165907670.png)

USER是指”进程执行用户“

PID是指”进程号“

%CPU是指”占用CPU的百分比“

%MEM是指”占用物理内存百分比“

VSZ是指”占用虚拟内存大小“

RSS是指”驻留集合大小，即进程所使用的非交换区物理内存大小“

TTY是指”终端信息“

STAT是指”当前运行状态“，S表示sleep（休眠)、r表示正在运行

START是指”执行的开始时间“

TIME是指”占用CPU时间“

COMMAND是指”进程名，也可以理解为执行该进程的指令“

<br>

**ps详解**

1.指令：ps -aux | grep xxx，比如想看看有没有sshd服务

2.指令说明

- System V 展示风格
- USER：用户名称
- PID：进程号
- %CPU：进程占用CPU百分比
- %MEM：进程占用物理内存的百分比
- VSZ：进程占用的虚拟内存大小（单位 ：KB）
- RSS：进程占用的物理内存大小（单位：KB）
- TTY：终端名称，缩写
- STAT：进程状态，其中S-睡眠，s-表示该进程是会话的先导进程，N-表示进程拥有比普通优先级更低的优先级，R-正在运行，D-短期等待，Z-僵死进程，T-被跟踪或者被停止等等
- STARTED：进程的启动时间
- TIME：CPU时间，即进程使用CPU的总时间
- COMMAND：启动进程所用的命令和参数，如果过长会被截断显示

**应用实例**

要求：以全格式显示当前所有的进程，查看进程的`父进程`。比如查看sshd的父进程信息：ps -ef | grep sshd

![image-20230716170731797](D:\typora图片\image-20230716170731797.png)

可以看出，sshd的父进程的PID为1，通过指令ps -aux | more可以看到PID为1的进程为

![image-20230716170808364](D:\typora图片\image-20230716170808364.png)

ps -ef是以全格式显示当前所有的进程

-e显示所有进程。-f 全格式

ps -ef | grep xxx

是BSD风格

UID：用户ID

PID：进程ID

PPID：父进程ID

C：CPU用于计算执行优先级的因子。数值越大， 表明进程是CPU密集型运算，执行优先级会降低；数值越小，表明进程是I/O密集型运算，执行优先级会提高

STIME：进程启动的时间

TTY：完整的终端名称

TIME：CPU时间

CMD：启动进程所用的命令和参数

<br>

## 终止进程kill和killall

若是某个进程执行一半需要停止时，或是已消了很大的系统资源时，此时可以考虑停止该进程。使用kill命令来完成此项任务。

**基本语法**

kill [选项] 进程号（功能描述：通过进程号终止进程）

killall 进程名称 （功能描述：通过进程名称终止进程，也支持通配符，这在系统因负载过大而变得很慢时很有用）

补充：

1.通过kill只会终止父进程，其子进程会交给1号进程接管。

2.通过killall终止进程，会将该进程下面的所有子进程也一并终止。

**常用选项**

-9：表示强迫进程立即停止（因为在有些情况下，系统处于保护机制，它会忽略kill指令，但是带-9会强制终止进程）

**应用案例**

案例1：踢掉某个非法登录用户：kill 对应进程号，比如kill 11421

案例2：终止远程登录服务sshd，在适当时候再次启动sshd服务：

```shell
kill sshd对应的进程号
/bin/systemctl start sshd.service # 再次重启sshd服务
```


案例3：终止多个gedit，演示killall：killall gedit

案例4：`强制`杀掉一个终端

kill 5480，没有反应。因为该终端正在工作，认为你可能是误操作。此时要终止必须要强制终止：kill -9 5480

<br>

## 查看进程树pstree

**基本语法**

pstree [选项] [进程号]，可以更加直观的来看进程信息，不写进程号，默认查看进程为1的进程树

**常用选项**

-p：显示进程的PID

-u：显示进程的所属用户

**应用实例：**

案例1：请以树状的形式显示进程pid：pstree -p （进程号）

案例2：请以树状的形式显示进程的用户：pstree -u

<br>

## 服务（service）管理

服务（service）本质就是进程，但是是运行在后台的，通常都会监听某个端口，等待其他程序的请求，比如（mysqld，sshd，防火墙等），因此我们又称为守护进程，是Linux中非常重要的知识点。下图为原理图。

![image-20230716172920953](D:\typora图片\image-20230716172920953.png)

补充：
1.SSHD，就是远程登陆服务，是通过SSH来登陆的。最后的一个符号D其实是daemon的简写，表示后台程序/守护进程/服务

**service管理指令**

1.service 服务名 [start | stop | restart | reload | status]

2.在CentOS7.0后，很多服务不再使用service，而是systemctl

3.在CentOS7.0后，被service指令管理的服务在/etc/init.d/查看（注意，必须最后带/）

**查看服务名：**

方式1：使用setup -> 系统服务 就可以看到全部：setup

下图中打星号的表示会随着linux的启动，自动启动。没有带星号的则需要手动启动。

将光标移动到某一个服务上，按空格，可以将自动启动的服务改为需要手动启动，即可以去掉星号。

按tab可以切换选择框。

![image-20230716173557061](D:\typora图片\image-20230716173557061.png)

方式2：/etc/init.d 看到service指令管理的服务：ls -l /etc/init.d

**服务的运行级别（runlevel）：**

Linux系统中有7种运行级别（runlevel）：**<font color="red">常用的是级别3和5</font>**

运行级别0：系统停机状态（一旦启动马上关机），系统默认运行级别不能设为0，否则不能正常启动

运行级别1：单用户工作状态，root权限，用于系统维护，禁止远程登录

运行级别2：多用户状态（没有NFS），不支持网络

运行级别3：完全的多用户状态（有NFS），无界面，登录后进入控制台命令行模式

运行级别4：系统未使用，保留

运行级别5：X11控制台，登录后进入图形GUI模式

运行级别6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动

开机的流程说明：

```bash
开机->BIOS->/boot->systemd进程1->运行级别->运行级对应的服务
```

**CentOS7后运行级别说明**

在/etc/inittab中进行了简化，如下：

multi-user.target: analogous to runlevel 3

graphical.target: analogous to runlevel 5

\# To view current default target, run:

systemctl get-default

\# To set a default target, run:

systemctl set-default TARGET.target

**chkconfig指令**

1.通过chkconfig命令可以给服务的各个运行级别设置自启动/关闭

2.chkconfig指令管理的服务在/etc/init.d查看

3.注意：Centos7.0后，很多服务使用systemctl管理

chkconfig基本语法

- 查看服务chkconfig --list [|grep xxx] (–list可加可不加）

  ![image-20230716175223326](D:\typora图片\image-20230716175223326.png)

  注意：sysv和systemd都是管理进程启动或关闭的程序，sysv启动服务慢，systemd启动服务快

- chkconfig 服务名 --list
- chkconfig --level 5 服务名 on/off：用这条指令可以设置某一个服务在某一个运行级别是自启动或者关闭自启动

案例演示：对network 服务 进行各种操作，把network在3这个级别关闭自启动

```bash
chkconfig --level 3 network off
chkconfig --level 3 network on
```

使用细节
chkconfig重新设置服务后自启动或关闭，需要重启机器reboot生效。

**systemctl管理指令**

1.基本语法：systemctl [start | stop | restart | status] 服务名

2.systemctl指令管理的服务在/usr/lib/systemd/system查看

**systemctl设置服务的自启动状态**

1.systemctl list-unit-files [| grep 服务名] （查看服务开机启动状态，grep可以进行过滤）

2.systemctl enable 服务名（设置服务开机启动）

3.systemctl disable 服务名（关闭服务开机启动）

4.systemctl is-enabled 服务名（查询某个服务是否是自启动的）

**应用案例：**

查看当前防火墙的状况，关闭防火墙和重启防火墙。

查看防火墙服务开机启动状态：systemctl list-unit-files | grep firewalld（.service可带可不带）

查看防火墙的状态：systemctl status firewalld

关闭防火墙：systemctl stop firewalld

重启防火墙：systemctl start firewalld

注意：

1.防火墙的服务名可以通过过滤查看/usr/lib/systemd/system文件来获得

2.static状态表示该服务与其他服务相关联，不能单独设置该服务的启动状态

3.与前面的chkconfig --level x 服务名 on/off不同的是，systemctl enable/disable 服务名的指令不需要带level，这是因为centos7.0以后，通过systemctl开启和关闭服务，默认是对级别3和5做操作，都生效。

**细节讨论：**

1.关闭或者启用防火墙后，立即生效。[`telnet`测试 某个端口即可]

可以通过`netstat -anp | more`指令查看网络状态

测试：telnet 192.168.200.130 111

![image-20230716180758053](D:\typora图片\image-20230716180758053.png)


无法连接，说明防火墙是启动的，并且没有把111端口打开。

当关闭防火墙后，再测试连接端口，则可以通过。

补充：

防火墙基本原理：可以认为系统在监听端口前面加了一个防火墙，当一个请求来的时候，如果相应端口是放开的，则请求可以通过，否则不通过。防火墙可以理解为筛子，通过预先设置的大小，来过滤掉不符合尺寸的服务。

开启telnet命令需要：依次点击‘开始’->“控制面板”->“程序”->“在程序和功能”找到并点击“启用或关闭windows功能”进入windows系统功能设置对话框。找到并勾选“Telnet客户端”和“Telnet服务器”

这种方式只是临时生效，当重启系统后，还是回归以前对服务的设置。

如果希望设置某个服务自启动或关闭永久生效，要使用systemctl [enable|disable] 服务名。

**打开或者关闭指定端口**

在真正的生产环境，往往需要将防火墙打开，但问题来了，如果我们把防火墙打开，那么外部请求数据包就不能跟服务器监听端口通讯。这时，需要打开指定的端口。

**firewall指令**

打开端口：firewall-cmd --permanent --add-port=端口号/协议

关闭端口：firewall-cmd --permanent --remove-port=端口号/协议

重新载入，才能生效：firewall-cmd --reload

查询端口是否开放：firewall-cmd --query-port=端口/协议

补充：要知道对应端口的协议是什么，可以使用`netstat -anp`指令查看

**应用案例：**

1.启用防火墙，测试111端口是否能telnet，答案是不行：通过firewall-cmd --query-port=111/tcp可以发现111端口是未开放的 `no`

2.开放111端口：

```bash
firewall-cmd --permanent --add-port=111/tcp
firewall-cmd --reload
firewall-cmd --query-port=111/tcp
```

3.再次关闭111端口：

```bash
firewall-cmd --permanent --remove-port=111/tcp
firewall-cmd --reload
firewall-cmd --query-port=111/tcp
```

<br>

## 动态监控进程

top与ps命令很相似。它们都用来显示正在执行的进程。top与ps最大的不同之处，在于top指令可以每隔一段时间就更新正在运行的进程。

**基本语法：** top [选项]

**选项说明：**

![image-20230716182051759](D:\typora图片\image-20230716182051759.png)

执行top指令：
![image-20230716182221502](D:\typora图片\image-20230716182221502.png)

说明：

```bash
20：10：57 	# 当前时间
4：47      	# 系统执行时间
2 users     # 目前在线的用户数
load average：0.00, 0.01, 0.05	#负载均衡：这三个值加起来除以3，如果在0.7以上，则负载较大，在0.7以下，则负载还行。
Tasks: 185 total,   1 running, 184 sleeping,   0 stopped,   0 zombie # 任务数，1个正在运行，231个休眠，0个终止，0个僵死
%Cpu(s):  1.3 us,  5.1 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st # us用户占用cpu；sy系统占用cpu；id空闲cpu比例
KiB Mem :  2027892 total,   998752 free,   567012 used,   462128 buff/cache # 共2G内存，大约1G空闲，用了0.5G，缓存占用0.5G
KiB Swap:  2097148 total,  2097148 free,        0 used.  1282572 avail Mem # swap分区，共2G，大约2G空闲，使用0个，可获取的内存1G多
12345678
```

注意：这些单位可以按E或e切换，切换为KB/MB/GB/EB/PB。

**交互操作说明：**

![image-20230716182820644](D:\typora图片\image-20230716182820644.png)

**应用实例**

案例1.监控特定用户，比如我们监控tom用户

top：输入此命令，按回车键，查看执行的进程。

u：然后输入“u”回车，再输入用户名tom，按回车键，即可

案例2：终止指定的进程，比如我们要结束tom登录

top：输入此命令，按回车键，查看执行的进程。

k：然后输入“k”回车，再输入要结束的进程ID号，按回车。此时一般会提示要输入信号量，输入9表示强制删除。

案例3：指定系统状态更新的时间（每隔10秒自动更新），默认是3秒：top -d 10

<br>

## 监控网络状态

**查看系统网络情况netstat**

基本语法：netstat [选项]

**选项说明**
-an 按一定顺序排列输出

-p 显示哪个进程在调用

![image-20230716183702368](D:\typora图片\image-20230716183702368.png)

说明：

proto：网络协议

Local Address：本地地址（是指linux地址)，如0.0.0.0:22：指有一个程序在本地监听22号端口

Foreign Address：外部地址

State：LISTEN表示监听，ESTABLISHED表示连接建立，TIME_WAIT表示超时等待

补充：
（1）0.0.0.0表示本地地址，127.0.0.1也是本地，：：：三个冒号是ipv6的形式显示的本地地址

```bash
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp6       0      0 :::22                   :::*                    LISTEN
12
```

这两条说明22号端口既可以接受ipv4的地址连接，也可以接受ipv6的地址连接

（2）

```bash
tcp        0     36 192.168.200.130:22      192.168.200.1:55605     ESTABLISHED
1
```

这条ESTABLISHED表示连接成功（建立连接）了，其中`192.168.200.130`是linux的本地地址，与外部地址`192.168.200.1`的55605端口发生连接

（3）只要要建立通信，必须要有端口，发送端端口是随机产生的，接收端端口相对是固定的。

（4）当tcp协议的一方断开连接时，另一方会认为可能是网络临时有问题，所以会等待，直到超时（一般是一分钟或者更久），此时状态是TIME_WAIT。

**应用案例**

请查看服务名为sshd的服务的信息：netstat -anp | grep sshd

**检测主机连接命令ping：**

是一种网络检测工具，它主要是用来检测远程主机是否正常，或是两部主机间的网线或网卡故障。

如：ping 对方ip地址

<br>

# RPM与YUM

## rpm包的管理

rpm用于互联网下载包的打包及安装工具，它包含在某些Linux分发版中。它生成具有.RPM扩展名的文件。RPM是RedHat Package Manager（RedHat软件包管理工具）的缩写，类似windows的setup.exe，这一文件格式名称虽然打上了RedHat的标志，但理念是通用的。

Linux的分发版本都有采用（suse，redhat，centos等等），可以算是公认的行业标准了。

**rpm包的简单查询指令**

查询已安装的rpm列表 rpm -qa | grep xx

举例：看看当前系统，是否安装了firefox：rpm -qa | grep firefox



**rpm包名基本格式**

一个rpm包名：`firefox-68.10.0-1.el7.centos.x86_64`

名称：firefox

版本号：68.10.0-1

适用操作系统：el7.centos.x86_64表示centos7.x的64位系统，如果是i686、i386表示32位系统，noarch表示通用



**rpm包的其他查询指令：**

rpm - qa：查询所安装的所有rpm软件包（query all）

rpm -q 软件包名：查询指定软件包是否安装

案例：rpm -q firefox

rpm -qi 软件包名：查询软件包信息

案例：rpm -qi firefox

rpm -ql 软件包名：查询软件包中的文件

比如：rpm -ql firefox

rpm -qf 文件全路径名 查询文件所属的软件包

rpm -qf /etc/passwd

**卸载rpm包：**

基本语法

rpm -e RPM包的名称（e表示单词erase）

应用案例

删除firefox 软件包：rpm -e firefox

细节讨论

1.如果其他软件包依赖于您要卸载的软件包，卸载时则会产生错误信息。
如：

$ rpm -e foo
removing these packages would break dependencies: foo is needed by bar-1.0-1
2.如果我们就是要删除foo这个rpm包，可以增加参数–nodeps，就可以强制删除，但是一般不推荐这样做，因为依赖于该软件包的程序可能无法运行
如：$ rpm -e --nodeps foo

**安装rpm包**

基本语法

rpm -ivh RPM包全路径名称

参数说明

i=install 安装

v=verbose提示

h=hash进度条

应用实例

演示卸载和安装firefox浏览器

```bash
rpm -e firefox # 卸载firefox浏览器
rpm -ivh /opt/firefox-68.10.0-1.el7.centos.x86_64.rpm # 先将安装包放在/opt目录下
```

<br>

## yum

Yum是一个Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包。

**yum的基本指令**

查询yum服务器是否有需要安装的软件

yum list | grep xx软件列表

**安装指定的yum包**

yum install xxx 下载安装

**yum应用实例：**

案例：请使用yum的方式来安装firefox

```bash
rpm -e firefox # 先卸载firefox
yum list | grep firefox # 如果有多个版本，都会显示出来
yum install firefox
```