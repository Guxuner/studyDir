### Linux学习大纲

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231012645413.png" alt="image-20201231012645413" style="zoom: 67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231013112160.png" alt="image-20201231013112160" style="zoom: 80%;" />

## 一、基础篇

Linux内核官网：https://www.kernel.org/

vm虚拟机官网下，激活码网上找（安装略）

centos安装：163镜像站找到centos下载7.6或者8.1的。（安装略）

### 1、网络连接的三种模式

#### 1、1桥接模式

![image-20201231133801393](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231133801393.png)

#### 1、2NAT模式

![image-20201231134355849](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231134355849.png)

#### 1、3主机模式

独立的系统不和外部发生联系

### 2、虚拟机的克隆

![image-20201231135052736](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231135052736.png)

### 3、快照管理

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231135728582.png" alt="image-20201231135728582" style="zoom:50%;" />

![image-20201231135600022](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231135600022.png)

### 4、虚拟机的迁移与删除

![image-20201231135940008](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231135940008.png)

### 5、安装vmtools（实现window与Linux的文件共享）

![image-20201231140335765](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231140335765.png)

![image-20201231140914397](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231140914397.png)

### 6、Linux的目录结构

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231141234998.png" alt="image-20201231141234998" style="zoom: 80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231141637933.png" alt="image-20201231141637933" style="zoom:67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231142004565.png" alt="image-20201231142004565" style="zoom:67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231142051135.png" alt="image-20201231142051135" style="zoom:67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231142436318.png" alt="image-20201231142436318" style="zoom:67%;" />

**菜鸟教程下的目录结构**：https://www.runoob.com/linux/linux-system-contents.html

## 二、实际操作篇

### 1、xshell与xftp远程连接软件

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231142730449.png" alt="image-20201231142730449" style="zoom:67%;" />

xshell与xftp下载地址：https://www.netsarang.com/zh/free-for-home-school/

### 2、常用指令

#### 2、1 vi与vim（Linux中的文本编辑器）

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231143710550.png" alt="image-20201231143710550" style="zoom:69%;" />

​	**vi与vim的三种模式与切换**！！！

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231143847655.png" alt="image-20201231143847655" style="zoom:67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231144123169.png" alt="image-20201231144123169" style="zoom: 84%;" />

vi与vim中的常用快捷键

**拷贝行：在一般模式中，n+yy，复制n行内容，p在光标所在的下一行粘贴。**

**删除行：在一般模式中，n+dd，删除光标所在行往下的n行（包括光标所在的行）。**

**查找：在命令模式下，/+需要查找的词。高亮显示，n查找下一个。**

**需要查找的高亮：在命令模式下，nohl取消高亮。**

**设置行号：在命令模式下，set nu**

**取消行号：在命令模式下，set nonu**

**去文档的最末尾：一般模式下，G**

**去文档的最首行：一般模式下，gg**

**撤销：，一般模式下，u**

**将光标移动到当前行的末尾：$**

**将光标到指定的行号：一般模式下，n + shift + g**

#### 2、2 开机、重启与用户注销

关机：shutdown	-h 	数字（多少分钟后关机）|now（现在关机）	|	halt

现在重启：shutdown	-r	now	|	reboot

将内存的东西同步到磁盘：sync，无论是重启还是关机，都使用该命令同步下，不然可能照成数据丢失。

用户注销：logout

#### 2、3 pwd

显示当前所在目录的绝对路径

#### 2、4 用户管理

Linux是一个多用户多任务的操作系统，任何一个要使用系统资源的用户，都需要向管理员申请一个账号，然后以这个账号进入系统。

**添加用户：useradd	[-d	目录名（指定家目录的位置）]	用户名**；在创建用的时候系统会在/home目录下创建一个与用户名相同的家目录与组。

**添加用户的时候指定组：useradd	-g	组名	用户名**

**修改用户的所在组：usermod	-g	组名	用户名**

**修改用户登入的初始目录：usermod	-d	目录名	用户名**

**给用户指定密码：passwd	用户名！！！**用户名要写不然只会修改当前登入用户密码

**删除用户：userdel	[-r 删除用户的同时删除其家目录]	用户名**，一般都是保留其家目录

**查询用户的信息：id	用户名**（用户id、组id、所在组）

**切换用户：su	用户名**（高权限用户到低权限用户不用密码，反之需要密码），**返回上一个用户：exit/logout**

**查看第一次登入用户的信息：who  am i**

#### 2、5、1 用户组管理

用户组类似角色，帮助系统对有共性与权限的多个用户进行统一管理，如：开发组、测试组。。。有什么权限

**添加组：groupadd	组名**

**删除组：groupadd	组名**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231155202257.png" alt="image-20201231155202257" style="zoom: 67%;" />

用户不能单独与组存在，每个用户不能独立于组外。

**系统中每个文件都有：所有者、所在组和其他组。**

谁创建了文件谁就是该文件的所有者

**修改文件的所有者：chown	[-R目录及目录下的所有文件修改所有者]	用户名	文件名**

**修改文件的所有者与文件的所在组：chown	[-R目录及目录下的所有文件修改所有者与组]	用户名：组名	文件名**

**修改文件的所在组：chgrp	[-R目录及目录下的所有文件修改所在组]	组名	文件名**

#### 2、5、2用户的权限管理

**每个文件都有所有者、所在组与其他组，在使用了ls查看当前目录的时候会显示如下的结果**

[**drwxrwxr-x. 2 dier dier 19 1月   1 01:14 studyDir**]()

------

**第0位表示文件的类型：l表示链接；d表示文件夹；-表示文件；c是字符设备文件（鼠标、肩旁）；b是块设备（如硬盘）**

**1-3位表示：文件所有者的权限。**

**4-6位表示：文件所在组成员的权限。**

**7-9位表示：文件其他组成员的权限。**

**之后的数字表示表示目录文件数+目录数，2表示当前目录下有一个文件。**

**之后表示所有者与所在组。**

**之后就是表示目录的大小字节数。**

**在之后最后的修改时间与文件名。**

------

**文件的权限分为rwx分别是读、写、执行**

**rwx在目录中的意义：**

**r：表示有这个权限的用户可以查看该目录的内容。**

**w：表示有这个权限的用户可以对目录的内容进行修改。**

**x：表示有这个权限的用户可以进入该目录。**

------

**rwx在文件中的意义：**

**r：表示有这个权限的用户可以查看该文件的内容**

**w：表示有这个权限的用户可以对文件的内容进行修改。**

**x：表示有这个权限的用户可以执行该文件。**

------

**修改权限：**

​	**第一种方式：chmod	u[+/-/=]	g[+/-/=]	o[+/-/=]	文件；如chmod	u=r+w+x	g=r+w	o=r	文件名**

​	**第二种方式：(rwx可以使用数字表达：4、2、1）chmod	777	文件；表示此文件的所有者、所在组成员、其他组成员都有读、写执行的权限。**



#### 2、6 Linux运行级别

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231155956826.png" alt="image-20201231155956826" style="zoom: 67%;" />

![image-20201231161101563](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231161101563.png)

**设置3的运行级别:systemctl 	set-default	multi-user.target（工作中主要使用的）**

##### 2、6、1如何使用运行级别找回root密码？

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231161818348.png" alt="image-20201231161818348" style="zoom:67%;" />

![image-20201231161920322](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231161920322.png)

![image-20201231162024409](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231162024409.png)

![image-20201231162051908](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231162051908.png)

![image-20201231162456192](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231162456192.png)

#### 2、7 帮助命令

**查看命令怎么使用：man	命令	|	help	命令	|	直接百度**

#### 2、8 文件类命令

**目录名可以是绝对目录也可以是相对目录**

**查看目录的内容：ls	[-a 查看所有 | -l 以列表的方式查看|-h人更好查看的方式]	[可以指定目录]	没有指定的话就是当前目录，也可以使用ll=ls -l**

**切换目录：cd	目录名；回到家目录cd	~；回到上一级目录cd	..**

**创建文件夹：mkdir	[-q	目录名；创建目录]	文件夹名称**

**删除文件夹：rmdir	文件名；如果文件夹有内容不能删除**

**递归强制删除文件：rm	-rf	文件名；**

**创建空文件：touch	文件名**

**拷贝文件：cp	源文件名	目标文件名**

**拷贝目录：cp	-r	源文件	目标文件;如果多次执行复制命令，会提示要覆盖前面的文件，\cp	源	目标表示强制覆盖**

**移动文件：mv	源文件	目标目录；**

**重命名：mv	源文件	目标文件；**

**查看（只能查看）文件内容：cat	[-n 显示行号]	文件名**

**管道命令：命令	|	命令；将前面执行的命令交给后面的命令执行。**	

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231201537060.png" alt="image-20201231201537060" style="zoom: 80%;" />

![image-20201231201705798](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231201705798.png)

**输出内容到控制台：echo	字符串|$HOSTNAME  输出主机名**

**展示文件的开头部分（默认文件的前10行）：head	-n	需要显示的行数	文件名**

**展示文件的结尾部分（默认文件后10行）：tail	[-n	需要显示行数	|	-f	文件名	实时追踪该文件，只要发生改变就显示]**

**将内容覆盖写入文件中：如：ls -l > 文件名；将输出到控制台的内容覆盖写入文件中；**

**将内容追加写入文件中：如：ls -l >> 文件名；将输出到控制台的内容追加写入文件中；**

**软连接（快捷键）：ln	-s	文件或者目录名给	软连接名；删除软连接的时候别rm -rf	软连接名这回将真正目录下的文件删除直接rm就行**

**查看历史使用过的指令：history	[数字；显示最近使用的哪几个指令]**

**执行之前使用的指令：！指令的行数，使用history查看历史使用指令的行号**

#### 2、9 日期指令

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231203319981.png" alt="image-20201231203319981" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20201231203411807.png" alt="image-20201231203411807" style="zoom: 80%;" />

#### 2、10 查找指令

**查看文件：find	[搜索范围]	[-name按文件名查找|-user按文件所有者查找|-size按大小查找（+大于，-小于，不写就是等于）单位k,M,G]**

**快速定位文件的位置：locale	文件名；执行前使用updatedb更新下数据库**

**定位指令在哪个目录下：which	命令**

**过滤查找：grep,通常与管道指令|一起使用；如cat	文件名|grep [-n显示行号|-i忽略大小写]	”hello“：显示文件中带有hello的行号并展示行号**

#### 2、11 压缩指令

**将文件压缩为.gz的格式：gzip	文件；并且是压缩的文件会消失**

**将.gz结尾的文件解压：gunzip	文件.gz**



**将目录压缩为.zip格式：zip	[-r递归压缩]	压缩文件名.zip	源目录**

**将.zip解压：unzip	[-d指定解压的目录]	文件.zip**



**tar指令：.tar.gz结尾的文件**

**压缩：tar	[-z|c：产生.tar打包文件文件|vf]	文件名	文件2	文件三；多个文件使用空格隔开**

**解压：tar	[-z：打包同时压缩|x：解tar包|v：显示详细信息|f：指定压缩后的文件名]	xx.tar.gz	【-C	指定解压的目录】**

### 3、定时任务调度

#### 3、1反复的任务调度crontab

任务调度：是指系统在某个时间执行的特定的命令或程序。

任务调度的分类：

​	1、系统工作：有些重要的工作必须反复的执行，病毒扫描等。

​	2、用户工作：用户在特定时间执行某些程序，如数据库的备份。

**任务调度基本语法：crontab	[-e编辑crontab定时任务|-l查询crontab任务|-r删除当前用户所有的crontab任务]**

**重启任务调度：service	crond	restart**

简单的一条指令直接写就行：*/1 * * * * cal >> /opt/to.txt

复杂的写sh脚本。

如果任务调度的权限不够会发送消息到/var/spool/mail/用户名下

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101124709262.png" alt="image-20210101124709262" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101125446341.png" alt="image-20210101125446341" style="zoom: 67%;" />

#### 3、2一次性的任务调度at

at命令是一次性的定时计划任务，at的守护进程atd会以后台模式运行，检查作业队列来运行。

默认情况下，atd每60秒检查作业队列，有作业时，会检查作业运行时间，如果时间与当前时间匹配则运行次作业。

**使用at指令的时候必须保证atd守护进程存在**

**at	[选项]	[时间]**

**查询at任务：atq**

**删除任务：atrm	任务编号**

**Ctrl + D结束at命令的输入。**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101131119959.png" alt="image-20210101131119959" style="zoom:67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101131218263.png" alt="image-20210101131218263" style="zoom:67%;" />

### 4、磁盘分区

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101132343645.png" alt="image-20210101132343645" style="zoom:67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101132836733.png" alt="image-20210101132836733" style="zoom:67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101133149682.png" alt="image-20210101133149682" style="zoom:67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101133219821.png" alt="image-20210101133219821" style="zoom: 96%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101133536693.png" alt="image-20210101133536693" style="zoom:68%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101142415307.png" alt="image-20210101142415307" style="zoom:70%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101142911980.png" alt="image-20210101142911980" style="zoom:67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101143523589.png" alt="image-20210101143523589" style="zoom: 80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101143650740.png" alt="image-20210101143650740" style="zoom:67%;" />

#### 4、1 查看系统磁盘与目录使用情况

**查看系统使用磁盘的情况：df 	-h**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101233212492.png" alt="image-20210101233212492" style="zoom: 80%;" />

#### 4、2工作常用指令介绍

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101234201924.png" alt="image-20210101234201924" style="zoom: 67%;" />

### 5、网络配置

#### 5、1 NAT网络原理图

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210101235709007.png" alt="image-20210101235709007" style="zoom:80%;" />

#### 5、2 查看虚拟机的网络配置信息

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102000139533.png" alt="image-20210102000139533" style="zoom: 80%;" />

**查看Linux种的网络IP与网关：ifconfig**

**windows：ipconfig**

**测试当前服务器是否能连接目的主机：ping	目的主机**

#### 5、3 网络配置

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102000743397.png" alt="image-20210102000743397" style="zoom:80%;" />

![image-20210102000917965](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102000917965.png)

**第二种方法使用后需要修改网络配置中（5.2图）的子网IP与NAT设置中的网关ip**

**之后重启网络服务或者重启系统**

**service	network	restart、reboot**

#### 5、4设置主机名与host映射

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102002332425.png" alt="image-20210102002332425" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102002406834.png" alt="image-20210102002406834" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102002919186.png" alt="image-20210102002919186" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102003400606.png" alt="image-20210102003400606" style="zoom:67%;" />

### 6、 进程管理

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102120916976.png" alt="image-20210102120916976" style="zoom:67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102121639880.png" alt="image-20210102121639880" style="zoom: 75%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102121709946.png" alt="image-20210102121709946" style="zoom:68%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102122013581.png" alt="image-20210102122013581" style="zoom: 70%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102123421022.png" alt="image-20210102123421022" style="zoom: 67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102123446141.png" alt="image-20210102123446141" style="zoom: 80%;" />

### 7、 服务管理

#### 7、1 查看服务

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102124725277.png" alt="image-20210102124725277" style="zoom: 67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102125429556.png" alt="image-20210102125429556" style="zoom:67%;" />

**使用tab键进入下面的确定与取消**

#### 7、2 开机自启与防火墙基本使用

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102130035285.png" alt="image-20210102130035285" style="zoom:67%;" />![image-20210102130228517](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102130228517.png)

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102131023963.png" alt="image-20210102131023963" style="zoom: 75%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102130419640.png" alt="image-20210102130419640" style="zoom: 67%;" />![image-20210102130815381](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102130815381.png)

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102131110637.png" alt="image-20210102131110637" style="zoom: 50%;" />

**设置开机启动与关闭服务：centos7之后没有指定运行级别默认是在3、5。服务名可以简写如：开启防火墙systemctl	start	firewalld也可以是systemctl	start	firewalld-service**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102134151445.png" alt="image-20210102134151445" style="zoom:67%;" />

**查看所有开放的端口：firewall-cmd	--list-all**

#### 7、3 动态监控

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102134649112.png" alt="image-20210102134649112" style="zoom:67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102135410805.png" alt="image-20210102135410805" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102140054840.png" alt="image-20210102140054840" style="zoom:67%;" />

### 8、 rpm与yum

#### 8、1 rpm

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102145414031.png" alt="image-20210102145414031" style="zoom: 50%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102145544487.png" alt="image-20210102145544487" style="zoom: 72%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102145701617.png" alt="image-20210102145701617" style="zoom: 67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102150500595.png" alt="image-20210102150500595" style="zoom:50%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102150710826.png" alt="image-20210102150710826" style="zoom:70%;" />

#### 8、2 yum

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102151112113.png" alt="image-20210102151112113" style="zoom:50%;" />

**yum	-y	install	软件名（在下载安装过程中不用手动的输入yes）**

**卸载：yum	-y	remove	软件名**

## 三、高级篇

### 1、定制 JavaEE环境

#### 1、1 JDK安装

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102151832870.png" alt="image-20210102151832870" style="zoom:67%;" />

#### 1、2 Tomcat安装

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210102152119051.png" alt="image-20210102152119051" style="zoom: 65%;" />

#### 1、3 MySQL安装（MySQL高级有讲）

------

### 2、shell编程

#### 2、1 基本介绍与变量

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103013811312.png" alt="image-20210103013811312" style="zoom: 60%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103014103915.png" alt="image-20210103014103915" style="zoom:77%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103120943757.png" alt="image-20210103120943757" style="zoom: 67%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103121616763.png" alt="image-20210103121616763" style="zoom:74%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103123835306.png" alt="image-20210103123835306" style="zoom:80%;" />

**shell编程中的多行注释			:<<!**

​													**内容**

​													**！**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103124945778.png" alt="image-20210103124945778" style="zoom: 63%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103125305960.png" alt="image-20210103125305960" style="zoom: 63%;" />

#### 2、2 运算符与判断

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103130706708.png" alt="image-20210103130706708" style="zoom:79%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103131320442.png" alt="image-20210103131320442" style="zoom: 73%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103131520890.png" alt="image-20210103131520890" style="zoom:76%;" />

**if	[	判断	]**

**then	**

**成功做啥**

**else**

​	**失败做啥**

**fi	结束**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103133313087.png" alt="image-20210103133313087" style="zoom: 64%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103133834471.png" alt="image-20210103133834471" style="zoom:73%;" />

#### 2、3 循环

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103134450812.png" alt="image-20210103134450812" style="zoom: 87%;" />

**for x in xxx：像foreach**

**SUM=0**
**for ((i=1; i<=100; i++ ))**	像平常那么写
**do**
        **SUM=$[$SUM+$i]**
**done**
**echo $SUM**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103134924185.png" alt="image-20210103134924185" style="zoom:92%;" />

**i=0**
**SUM=0**
**while [ $i -le $1 ]**	while后有空格，条件判断语句开口结尾有空格
**do**
        **SUM=$[$SUM+$i]**
        **i=$[$i+1]**
**done**
**echo $SUM**

#### 2、4 读取控制台的输入

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103140653237.png" alt="image-20210103140653237" style="zoom:77%;" />

**read -p "请输入一个数字=" NUM**
**echo 你输入的数字是：$NUM**
**read -t 5 -p "请输入第二个数字" num2**
**echo 你输入的第二个数字：$num2**

#### 2、5 函数

**系统函数：**

**basename /home/aaa/test.txt**
**test.txt**

**dirname /home/aaa/test.txt**
**/home/aaa**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103140954918.png" alt="image-20210103140954918" style="zoom: 73%;" />

**自定义函数：**

**function getSum(){**
        **SUM=$[$n1+$n2]**
        **echo "和位："$SUM**
**}**

**read -p "请输入n1" n1**
**read -p "请输入n2" n2**

**getSum $n1 $n2**

![image-20210103141818805](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103141818805.png)

#### 2、6 定时备份数据库

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103142846265.png" alt="image-20210103142846265" style="zoom:76%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103145417515.png" alt="image-20210103145417515" style="zoom: 52%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103145558662.png" alt="image-20210103145558662" style="zoom: 56%;" />

**之后给sh文件执行的权限加上定时任务调度即可。**

### 3、centos7与centos8区别

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103153634645.png" alt="image-20210103153634645" style="zoom:80%;" />

### 4、 日志管理（后再说）

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103153753157.png" alt="image-20210103153753157" style="zoom: 50%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103153919607.png" alt="image-20210103153919607" style="zoom: 52%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103154034856.png" alt="image-20210103154034856" style="zoom:54%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103154337261.png" alt="image-20210103154337261" style="zoom:58%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103154538507.png" alt="image-20210103154538507" style="zoom:52%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103155342253.png" alt="image-20210103155342253" style="zoom:65%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103155426366.png" alt="image-20210103155426366" style="zoom:60%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103155708974.png" alt="image-20210103155708974" style="zoom: 54%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210103155950977.png" alt="image-20210103155950977" style="zoom:52%;" />