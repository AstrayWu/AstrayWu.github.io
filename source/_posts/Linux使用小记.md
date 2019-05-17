---
title: Linux使用小记
date: 2019-04-30 00:45:45
tags: Linux
---

近期需要回顾一些基础知识，需要相对系统地学习一下。

## ref

- [Linux工具快速教程](https://linuxtools-rst.readthedocs.io/zh_CN/latest/index.html) 主要参考的文档

## 帮助

- 在只记得部分命令关键字的场合，我们可通过`man -k`来搜索；
- 需要知道某个命令的简要说明，可以使用`whatis`；用`whatis -w "loca*"实现正则匹配；
- 而更详细的介绍，则可用`info`命令；
- 查看命令在哪个位置，我们需要使用`which`；当系统中安装了同一软件的多个版本时，不确定使用的是哪个版本时，这个命令就能派上用场；
- 而对于命令的具体参数及使用方法，我们需要用到强大的`man`；

在man的帮助手册中，将帮助文档分为了9个类别，对于有的关键字可能存在多个类别中， 我们就需要指定特定的类别来查看；（一般我们查询bash命令，归类在1类中）；

1. **用户可以操作的命令或者是可执行文件**
2. 系统核心可调用的函数与工具等
3. **一些常用的函数与数据库**
4. 设备文件的说明
5. 设置文件或者某些文件的格式
6. 游戏
7. 惯例与协议等。例如Linux标准文件系统、网络协议、ASCⅡ，码等说明内容
8. 系统管理员可用的管理条令
9. 与内核有关的文件

我们看到printf在分类1和分类3中都有；分类1中的页面是命令操作及可执行文件的帮助；而3是常用函数库说明；如果我们想看的是C语言中printf的用法，可以指定查看分类3的帮助：

```sh
man 3 printf
```

## 文件及目录

```sh
# 查看当前目录下文件个数
find ./ | wc -l

# 复制目录
cp -r source_dir  dest_dir

# 切换到home目录
cd
cd ~

# 按时间排序，新的在前
ls -lt
ls -ltr

# 给每项文件前面增加一个id编号
ls | cat -n

# 搜寻文件或目录:
find ./ -name "core*" | xargs file # xargs是列出文件类型

# 查找目标文件夹中是否有obj文件:
find ./ -name '*.o'

# 递归当前目录及子目录删除所有.o文件
find ./ -name "*.o" -exec rm {} \;
```

find是实时查找，如果需要更快的查询，可试试locate；locate会为文件系统建立索引数据库，如果有文件更新，需要定期执行更新命令来更新索引库

**cat vi head tail more**

```sh
# 显示时同时显示行号
cat -n

# 显示文件第一行:
head -1 filename

# 显示文件倒数第五行:
tail -5 filename

# 查看两个文件间的差别:
diff file1 file2

# 动态显示文本最新信息:
tail -f crawler.log

ln cc ccAgain # 硬连接；删除一个，将仍能找到；
ln -s cc ccTo # 符号链接(软链接)；删除源，另一个无法使用；（后面一个ccTo 为新建的文件）
```

批处理命令连接执行，使用 |
串联: 使用分号 ;
前面成功，则执行后面一条，否则，不执行:&&
前面失败，则后一条执行: ||

```sh
ls /proc && echo  suss! || echo failed.
```

### 查看文件夹大小

[linux下使用 du查看某个文件或目录占用磁盘空间的大小](https://www.jianshu.com/p/936ae380de54)

```sh
# -h：以人类可读的方式显示
# -s：显示目录占用的磁盘空间大小，不显示其下子目录和文件占用的磁盘空间大小
# 查看当前目录总共占的容量。而不单独列出各子项占用的容量
du -sh

# 查看当前路径下一级子目录的占用大小（不包含文件）
du -h --max-depth=1

# 包含文件和文件夹
du -ah --max-depth=1

```

## 环境变量

参考[Linux下的环境变量](https://blog.csdn.net/jiangyanting2011/article/details/78875928)

Linux中环境变量包括系统级和用户级，系统级的环境变量是每个登录到系统的用户都要读取的系统变量，而用户级的环境变量则是该用户使用系统时加载的环境变量。所以管理环境变量的文件也分为系统级和用户级的。

1. 系统级
   1. `/etc/environment`: 是系统在登录时读取的第一个文件，**用于为所有进程设置环境变量。**
   2. `/etc/profile`：是系统登录时执行的第二个文件，可以用于设定针对全系统所有用户的环境变量。该文件一般是调用`/etc/bash.bashrc`文件。
   3. `/etc/bash.bashrc`：系统级的`bashrc`文件，为每一个运行bash shell的用户执行此文件。此文件会在用户每次打开shell时执行一次。
2. 用户级
   1. `~/.profile`: 是对应当前登录用户的profile文件，用于定制当前用户的个人工作环境。 每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次!默认情况下,他设置一些环境变量,执行用户的.bashrc文件。
   2. `~/.bashrc`: 是对应当前登录用户的bash初始化文件，当用户每次打开shell时，系统都会执行此文件一次。平时设置这个文件就可以了。

那么根据以上描述，这几个文件的执行先后顺序应当是：
`/etc/enviroment –>/etc/profile –>~/.profile –>/etc/bash.bashrc–> ~/.bashrc`

配置环境变量

1. 临时生效
    直接运行export命令定义变量【只对当前shell(BASH)有效(临时的)】

    ```sh
    export PATH=/usr/local/webserver/php/bin:$PATH
    ```

2. 对当前用户有效
   编辑`~/.bashrc`并添加`export PATH=$PATH:/usr/local/webserver/php/bin`；用`source ~/.bashrc`让修改马上生效
3. 对所有用户生效
    在`/etc/profile`文件中添加变量，并执行`source /etc/profile`

## 文件与目录权限修改

```sh
chown # 改变文件的拥有者
chmod #改变文件读、写、执行等属性

# 递归子目录修改
chomd chown -R tuxapp source/
chown 777 -R test_dir
```

`chmod`的语法为

```sh
chmod [who] [+ | - | =] [mode] 文件名
```

- u 表示“用户（user）”，即文件或目录的所有者。
- g 表示“同组（group）用户”，即与文件属主有相同组ID的所有用户。
- o 表示“其他（others）用户”。
- a 表示“所有（all）用户”。它是系统默认值。



```sh
# 在一个命令行中可给出多个权限方式，其间用逗号隔开
chmod g+r，o+r example # 使同组和其他用户对文件example 有读权限

# 对所有用户增加执行权限
chmod a+x sort
```

## 文本处理

### find 文件查找

```sh
# 查找txt和pdf文件:
find . \( -name "*.txt" -o -name "*.pdf" \) -print

# 正则方式查找.txt和pdf:
# -iregex： 忽略大小写的正则
find . -regex  ".*\(\.txt|\.pdf\)$"

# 否定参数 ,查找所有非txt文本:
find . ! -name "*.txt" -print

# 指定搜索深度,打印出当前目录的文件（深度为1）:
find . -maxdepth 1 -type f

## 定制搜索
# 按类型搜索
find . -type d -print  //只列出所有目录
# -type f 文件 / l 符号链接 / d 目录

# file命令可以检查文件具体类型（二进制或文本），find不行
# $ file redis-cli  # 二进制文件
# redis-cli: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.9, not stripped
# $ file redis.pid  # 文本文件
# redis.pid: ASCII text

# 可以用以下命令组合来实现查找本地目录下的所有二进制文件:
ls -lrt | awk '{print $9}'|xargs file|grep  ELF| awk '{print $1}'|tr -d ':'

# 按时间搜索
# -atime 访问时间 (单位是天，分钟单位则是-amin，以下类似）
# -mtime 修改时间 （内容被修改）
# -ctime 变化时间 （元数据或权限变化）

# 最近第7天被访问过的所有文件:
find . -atime 7 -type f -print

# 最近7天内被访问过的所有文件:
find . -atime -7 -type f -print

# 查询7天前被访问过的所有文件:
find . -atime +7 type f -print

# 按大小搜索
# w字 k M G 寻找大于2k的文件:
find . -type f -size +2k

# 删除当前目录下所有的swp文件:
find . -type f -name "*.swp" -delete
find . -type f -name "*.swp" | xargs rm

# 执行动作（强大的exec）
# 将当前目录下的所有权变更为weber:
find . -type f -user root -exec chown weber {} \;
# {}是一个特殊的字符串，对于每一个匹配的文件，{}会被替换成相应的文件名

# 将找到的文件全都copy到另一个目录:
find . -type f -mtime +10 -name "*.txt" -exec cp {} OLD \;

# 如果需要后续执行多个命令，可以将多个命令写成一个脚本。然后 -exec 调用时执行脚本即可:
-exec ./commands.sh {} \;
```

### grep 文本搜索

常用参数

- -o 只输出匹配的文本行 VS -v 只输出没有匹配的文本行
- -c 统计文件中包含文本的次数
    `grep -c “text” filename`
- -n 打印匹配的行号
- -i 搜索时忽略大小写
- -l 只打印文件名

```sh
# 在多级目录中对文本递归搜索(程序员搜代码的最爱）:
grep "class" . -R -n

# 匹配多个模式:
grep -e "class" -e "vitural" file
```

## 关机

[linux定时关机](http://www.cnblogs.com/deeryao/archive/2010/09/10/1823665.html)

```sh
# 立刻关机
shutdown -h now

# 在23点定时关机
shutdown -h 23:00

# 延时关机sudo shutdown +minutes（这个加号不可省略，minutes表示分钟）
# 两小时候后关机
shutdown -h +120

# 取消前一个关机命令按“Ctrl+C”键或输入命令：
shutdown -c
```