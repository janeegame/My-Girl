LinuxNotes
=====================


# 1. 用户

## 1.1 创建新用户

```
$ sudo useradd myadmin  //只创建用户
$ sudo passwd mypasswd  //设置密码
$ sudo adduser myadmin  //新建用户完整版
```

## 1.2 创建用户组

```
$ groups myadmin  //必须要有一个用户作为用户组组长
```

## 1.3 将其他用户加入用户组(需要权限)

```
$ sudo usermod -G mygroup myadmin
```

## 1.4 用户组管理信息

```
$ man usermod
```

## 1.5 删除用户

```
$ sudo deluser myadmin
```

## 1.6 改变文件的所有者

```
$ sudo chown myadmin myfile
```

--------

# 2. 文件

## 2.1 目录切换

- . 表示当前目录
- .. 表示上级目录
- - 表示上一次所在目录
- ~ 通常表示当前用户的home目录

```
$ cd ..  //切换到上一级目录
$ cd -  //切换到上一次所在目录
$ cd ~  //切换到home目录
```

## 2.2 创建空目录

```
$ mkdir mydir
```

## 2.3 同时创建多级目录

```
$ mkdir -p mypath
```

## 2.4 创建文件

```
$ touch myfile
```

## 2.5 复制一个文件到指定目录

```
$ cp myfile mypath
```

## 2.6 复制一个目录(递归复制)

```
$ cp -r mydir mynewdir
```

## 2.7 删除文件

```
$ rm myfile
```

## 2.8 强制删除

```
$ rm -f myfile
```

## 2.9 删除目录(递归删除)

```
$ rm -r mydir
```

## 2.10 移动文件

```
$ mv myfile mydir
```

## 2.11 重命名文件

```
$ mv myfile mynewname
```

## 2.12 查看文件

>常用查看

- cat myfile 为正序显示
- tac myfile 为倒序显示
- nl 命令,添加行号并打印,比 cat -n 命令更专业
- nl -b a myfile 表示无论是否为空行,同样列出行号
- nl -b t myfile 只列出非空行的编号(默认为这种方式)
- nl -n ln myfile 在行号字段最左端显示
- nl -n rn myfile 在行号字段最右端显示,且不加0
- nl -n rz myfile 在行号字段最右端显示,且加0
- nl -w myfile 行号字段占用的位数(默认为6位)

>分页查看

- more myfile  //只能向一个方向滚动
- less myfile

>部分查看

- head myfile  //查看头10行
- tail myfile //查看尾10行
- tail -n [num] myfile  //查看尾num行
- tail -f myfile  //不停地读某个文件的内容并显示.用于动态查看日志

>查看文件类型

- file myfile

## 2.13 搜索文件

- whereis myfile
- locate myfile
- which myfile
- find mydir -option myfile

--------

## 3. 环境变量

## 3.1 环境变量分类

- 当前Shell进程私有用户自定义变量,如上面我们创建的tmp变量,只在当前Shell中有效.
- Shell本事内建的变量.
- 从自定义变量导出的环境变量.

_也可以用全局环境变量和局部环境变量进行划分_

```
$ set  //显示当前Shell所有变量,包括其内建环境变量(与Shell外观等相关),用户自定义变量及导出的环境变量.

$ env  //显示与当前用户相关的环境变量,还可以让命令在指定环境中运行.

$ export  //显示从Shell中导出成环境变量的变量,也能通过它将自定义变量导出为环境变量.
```

## 3.2 添加自定义路径到"PATH"环境变量

```
$ PATH=$PATH:/home/mypath
```
注意这里一定要使用绝对路径,之后就可以在任意目录执行那两个命令了.

## 3.3 删除已有变量

```
$ unset myvar
```

## 3.4 让环境变量立即生效

```
$ source myvar
```
zsh配置文件home目录下的 .zshrc

```
$ source .zshrc
```

source命令还有一个别名就是 . ,上面的命令如果替换成 . 方式就该是:

```
$ . ./.zshrc       //注意第一个 . 后面有空格
```
--------
## 4. 文件打包与解压缩

## 4.1 使用zip打包文件夹

```
$ zip -r -q -o myfile.zip mypath
```

其中,

 -r 表示递归打包包含子目录的全部内容.

 -q 表示安静模式,不向屏幕输出信息.

 -o 表示输出文件,需要在后面紧跟输出文件名.

## 4.2 使用 du 查看打包后的文件大小

```
$ du -h -d myfile.zip ~ | sort
```

其中,
 -h 表示  human_readable, 易读模式.

 -d 表示 max_depth, 所查看文件深度.

## 4.3 使用 unzip 解压缩文件

```
$ unzip mufile.zip
```

其中,unzip 后可以加参数

 -q 表示使用安静模式.

 -l 表示不解压只是看压缩文件的内容.

 -o 表示指定编码类型.

## 4.4 使用 tar 打包工具

>创建tar包

```
$ tar -cf myfile.tar mypath
```

其中,

-c 表示创建一个tar包文件.

-f 用于指定创建的文件名,文件名必须紧跟在-f 参数之后.


>解压一个文件到指定路径已存在的目录

```
$ tar -xf shiyanlou.tar -C mydir
```

_注意要先用 mkdir mydir 创建 mydir._

>只查看不解压文件

```
$ tar -tf shiyanlou.tar
```

--------

## 5. 使用crontab

## 5.1 crontab简介

通常，crontab 储存的指令被守护进程激活，crond 为其守护进程，crond 常常在后台运行，每一分钟会检查一次是否有预定的作业需要执行。

## 5.1 crontab准备

> 更新和启动

```
$ sudo apt-get install -y rsyslog
$ sudo service rsyslog start
$ sudo cron -f &
```

## 5.2 crontab使用
```
$ crontab --help
```

--------

# 6. 顺序执行多条命令

## 6.1 执行多条命令

>用分号隔开

```
$ sudo apt-get update; sudo apt-get install some-tool; some-tool
```

## 6.2 查看是否有执行失败的命令

```
$ echo $?
```

若返回 1, 则说明上条命令执行失败;

若返回 0, 则说明上条命令执行成功.

## 6.3 顺序执行&&

在 && 符号前,若命令

执行成功,则执行&&后面的命令,

执行失败,则不执行&&后面的命令.

## 6.4 顺序执行||

在 || 符号前,若命令

执行成功,则执行||后面的命令,

执行失败,则不执行&&后面的命令.

--------

# 7. 管道

## 7.1 管道的用法

通过管道将前一个命令(ls)的输出作为下一个命令(less)的输入，然后就可以一行一行地看

```
$ ls -al mypath | less
```

## 7.2 其他可以和管道配合的命令

cut 命令: 打印每一行的某个字段.

grep 命令: 在文本中或stdin中查找匹配字符串.

wc 命令: 简单小巧的计数工具.

sort 命令: 排序.

uniq命令: 去重.

--------

# 8. 文本处理命令

## 8.1 tr命令

>option

-d 表示删除匹配的字符,注意不是全词匹配也不是按字符顺序匹配;

-s 表示去除指定的在输入文本中连续并重复的字符;

```
$ echo 'mystring' | tr -d 'mychar'
$ echo 'mystring' | tr -s 'mychar'
$ echo 'mystring' | tr '[:lower:]' '[:upper:]'\
$ echo 'mystring' | tr '[a-z]' '[A-Z]'
```

## 8.2 col命令

>option

-x 表示将Tab转换为空格;

-h 表示将空格转换为Tab(默认选项);

```
$ cat -A mypath
$ cat mypath | col -x | cat -A
```

## 8.3 join命令

>option

-t 指定分隔符,默认为空格;

-i 忽略大小写差异;

-1 指明第一个文件要用哪个字段来对比,默认对比第一个字段;

-2 指明第二个文件要用哪个字段来对比,默认对比第一个字段;

## 8.4 paste命令

该命令在不对比数据的情况下,简单地将多个文件合并在一起,以Tab隔开.

>option

-d 指定合并地分隔符,默认为Tab;

-s 不合并到一行,每个文件为一行;

--------

# 9. 重定向

重定向就是将原本输出的数据重定向到一个文件中.

>数据流重定向

```
$ echo 'mystring' > redirect
$ echo 'mystring' >> redirect
$ cat redirect
```

# 9.1 简单的重定向

> Linux默认提供了三个特殊设备


| 文件描述 | 设备文件 | 说明 |
|---------|----------|-----|
| 0 | /dev/stdin | 标准输入 | 
| 1 | /dev/stdout | 标准输出 |
| 2 | /dev/stderr | 标准错误 | 


>我们可以这样使用这些文件描述符

默认使用终端的标准输入作为命令的输入,标准输出作为命令的输出

```
$ cat
(按 Ctrl+C 退出)
```

将一个文件作为命令的输入,标准输出作为命令的输出

```
$ cat Documents/test.c
```

将 echo命令通过管道传过来的数据作为 cat命令的输入,将标准输出作为命令的输出

```
echo 'hi' | cat
```

将 echo命令的输入从默认的标准输出重定向到一个普通文件

```
$ echo 'hello' > redirect
$ cat redirect
```

_注意:管道默认是连接前一个命令的输出到下一个命令的输入,而重定向通常需要一个文件来建立两个命令的连接_

## 9.2 使用tee命令同时重定向到多个文件

有时候,除了需要将输出重定向到文件,也需要将信息打印在终端,就可以用tee命令来实现

```
$ echo 'hello DemCharlie' | tee myfile
```

## 9.3 永久重定向

exec命令的作用是使用指定的命令替代当前的 Shell, 直到使用一个进程替代当前进程,或者指定新的重定向

```
$ zsh //先开启一个子 Shell
$ exec 1>myfile  //使用exec替换当前进程的重定向,将标准输出重定向到一个文件
                 //后面你执行的命令都是将被重定向到文件中,直到你推出当前子 Shell,或取消exec重定向
$ ll
$ exit
$ cat myfile
```

## 9.4 使用xargs分割参数列表

xargs命令在有些时候十分有用,特别是当用来处理大量输出结果的命令如 find, locate, grep的结果,详细用法请参看man文档

```
$ cut -d: -f1 < /etc/passwd | sort | xargs echo
```

上面这个命令用于将/etc/passwd文件按 : 分割取第一个字段排序后,使用 echo命令生成一个列表.

--------

# 10. 正则表达式

## 10.1 基本语法

- +表示前面的字符必须出现至少一次,例如，例如，"goo+gle",可以匹配"gooogle","goooogle"等；

- ?表示前面的字符出现最多一次,例如，"colou?r",可以匹配"color"或者"colour";

- \*表示前面的字符可以出现零次,一次,或多次,例如，"colou?r",可以匹配"color"或者"colour";

- ^表示匹配字符串开始的位置;

- $表示匹配字符串结束的位置;

- {n}表示确定匹配 n次, n是一个非负整数;

- {n,}表示至少匹配 n次, n是一个非负整数;

- x|y 表示匹配 x或者y;

- [xyz] 表示字符集合,匹配所包含的任意一个字符;

- [^xyz] 表示排除型字符集合,匹配未列出的任意字符,'补集';

## 10.2 常用工具

- grep 模式匹配命令

- sed 流编辑器

- awd 文本处理

--------

# 11. 进程概念

## 11.1 基本概念

- 程序 procedure
- 进程 process
- 线程 thread

![avatar](https://dn-simplecloud.shiyanlou.com/1135081469062947147-wm)

## 11.2 进程的特性

- 动态性:
- 并发性:
- 独立性:
- 异步性:
- 结构性:

## 11.3 进程的分类

> 以进程的功能与服务的对象来分:

- 用户进程:
- 系统进程:

> 以应用程序的服务类型来分:

- 交互进程
- 批处理进程
- 守护进程

## 11.4 进程的衍生

- 子进程
- 僵尸进程 zombie
- 孤儿进程
- 祖先进程 init

查看进程结构

```
$ pstree
```
ps 全写是(process status).

## 11.5 进程组与Sessions

PID : 进程的ID 

PGID : 进程组的ID

每一个进程组都会在一个Session中,并且这个Session是唯一存在的.

Session主要是针对一个tty建立,Session中的每一个进程都称为一个工作job.

Session的意义在于将多个jobs囊括在一个终端,并取其中一个job作为前台,来直接接受该终端的输入输出以及终端信号.其他jobs在后台运行

foreground : 前台,就是在终端中运行,能交互的.

background : 后台,就是在终端中运行,但不能交互,也不会显示其执行过程.

## 11.6 工作管理

当一个进程在前台运作时我们可以用 Ctrl + C 来终止它，但是若是在后台的话就不行了.

通过 & 符号,让命令在后台中运行

```
$ ll &
```

通过 Ctrl + Z 使当前工作停止并丢到后台中去. 

使用这个命令来查看被停止并放置在后台的工作

```
$ jobs
```

将后台的工作拿到前台来

```
fg 
fg %jobnumber
```

不同于 Ctrl + Z,若想让工作在后台运行,使用这个命令

```
bg
bg  %jobnumber
```

删除一个 job 或 process 

```
kill -signal %number
```

其中number是 job的 ID时 删除 job; 是 PID时 删除 process.

常用的信号值

| 信号值 | 作用 |
| ---- | ---- |
| -1 | 重新读取参数运行,类似于restart |
| -2 | 如同 Ctrl + C 的操作推出 |
| -9 | 强制终止该任务 |
| -15 | 正常的方式终止该任务 |

--------

# 12. 管理控制

## 12.1 top工具的使用

top是一个前台执行的程序,所以执行后便进入到一个交互界面.

```
$ top
```

| 常用交互命令 | 解释 | 
| ---- | ---- |
| Q | 退出程序 |
| L | 切换显示平均负载和启动时间的信息 |
| P | 根据CPU使用百分比大小进行排序 |
| M | 根据驻留内存大小进行排序 |
| I | 忽略闲置和僵死的进程,这时一个开关命令 |
| K | 终止一个进程,系统提示输入PID及发送的信号值,安全模式下该命令被屏蔽 |

## 12.2 ps工具的使用

所有进程信息

```
$ ps aux
```

这个命令可以破欸和grep正则表达式一起使用

```
pa aux | grep zsh
```

此外我们还可以查看时,将连同部分的进程呈树状显示出来

```
$ ps axjf
```

自定义需要的参数显示

```
$ ps -afxo user,ppid,pid,pgid,command
```

查看所有进程以及它们的相关性

```
$ pstree
```

## 12.3 进程的管理

使用kill关闭进程

``
$ kill -9 mypid
``

## 12.4 进程的执行顺序

进程的调度队列通过进程的优先级值来排序,优先级值越小越优先!

有两个优先级值变量

> nice : 静态优先级,范围[-20,19];普通用户只可以调制属于自己的进程,并且其使用的范围只能是[0,19];

> PR(Priority) : 动态优先级,是进程在内核中实际的优先级值;Linux实际上实现了140各优先级范围,取值[0,139],其中0\~99是实时进程的值,而100\~139是给用户的;PR中100~139有对应关系 PR = 120 + nice

修改已经存在的进程的优先级

```
$ renice -5 mypid
```

--------

# 13. 日志系统

## 13.1 日志分类

- 常见的日志
- 配置的日志
- 轮替的日志

## 13.2 常见的日志

常见的日志
        |__系统日志
        |__应用日志

查看有哪些日志

```
ll /var/log
```

查看日志中的信息

```
$ less auth.log

$ less /var/log/apt/history.log

$ less /var/log/apt/term.log
```

## 13.3 配置的日志

日志的产生

- 一种是由软件开发商自己来自定义日志格式然后指定输出日志位置;
- 一种方式就是Linux提供的日志服务程序,如syslog,rsyslog,

syslog 是一个系统日志记录程序,rsyslog 是它的加强版,全称是 rocket-fast system for log;

手动开启rsyslog服务

```
$ sudo apt-get update
$ sudo apt-get install -y rsyslog
$ sudo service rsyslog start
$ ps aux | grep syslog
```

既然 rsyslog 是一个服务,那么它便是可以配置,为我们提供一些我们自定义的服务,首先我们来看 rsyslog 的配置文件是什么样子的,而 rsyslog 的配置文件有两个

- 一个是 /etc/rsyslog.conf
- 一个是 /etc/rsyslog.d/50-default.conf

第一个主要是配置的环境,也就是 rsyslog 加载什么模块,文件的所属者等;而第二个主要是配置的 Filter Conditins

```
$ vim /etc/rsyslog.conf

$ vim /etc/rsyslog.d/50-default.conf
```

## 13.4 转储的日志

logrotate 程序是一个日志文件管理工具.用来把旧的日志文件删除,并创建新的日志文件.我们可以根据日志文件的大小,也可以根据其天数来切割日志,管理日志,这个过程又叫做'转储';

显而易见,logrotate 是基于 CRON 来运行的,其脚本是 /etc/cron.daily/logrotate ; 同时我们可以在 /etc/logrotate 中找到其配置文件

```
$ cat etc/logrotate.conf
```

--------

# The Funny Things

## 输出图像字符

安装

```
$ sudo apt-get update;
$ sudo apt-get install sysvbanner
```

执行

```
$ banner mychar
$ printerbanner -w 50 mychar
```

其中,printerbanner可以自定义字体, -w 表示指定宽度.

## 数字雨

安装

```
$ sudo apt-get update; 
$ sudo pat-get install cmatrix
```

执行

```
$ cmatrix
```

## 火炉

安装

```
$ sudo apt-get install libaa-bin
```

执行

```
$ aafire
```

## 太空侵略者 Space Invaders

安装

```
$ sudo apt-get install ninvaders
```

运行

```
$ /usr/games/ninvaders
```

## 彩色caca

安装

```
$ sudo apt-get install caca-utils
```

运行

```
$ cacademo
$ cacafire
$ cacaview mypicturefile
```

## 游戏bb

安装

```
$ sudo apt-get update
$ sudo apt-get install bb
```

执行

```
$ /usr/games/bb
```
