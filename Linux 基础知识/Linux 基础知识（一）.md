##1. Linux 简介
Linux 的主要应用领域：  
1. 基于 Linux 的企业服务器  
2. 嵌入式应用

Linux 的版本分为两个：Linux 内核版本和 Linux 发行版本。
内核版本地址：[https://www.kernel.org/](https://www.kernel.org/)

发行版本有很多，常见一些的如下图所示：
![Linux 的一些发行版本](http://img.blog.csdn.net/20160417143718045)

PS: 关于 Linux 的版本号，以 2.6.18 为例，分别表示：主版本号，次版本号，末版本号（根据更新的大小来改变不同的版本号）。

Linux 下常见的开源软件：
![Linux 常用开源软件](http://img.blog.csdn.net/20160417144318476)

Linux 的特点：  
1. 严格区分大小写  
2. Linux 中所有内容以文件格式存在，包括硬件（即：一切皆文件）。


##2. Linux 入门知识
- 扩展名

Linux 没有扩展名的概念，下面这些带扩展名的只是便于管理员识别文件类型。
![扩展名](http://img.blog.csdn.net/20160417145907284)

Windows 下的程序不能【直接】在 Linux 下运行。

- 分区

Linux 下的分区分为三种：主分区、扩展分区和逻辑分区。

主分区：最多有 4 个。

扩展分区：  
1. 最多只能有 1 个；  
2. 主分区加扩展分区最多有 4 个；   
3. 不能写入数据，只能包含逻辑分区。

![必须分区](http://img.blog.csdn.net/20160417152036511)

- 格式化

格式化（高级格式化）又称逻辑格式化，它是指根据用户选定的文件系统（如FAT16、FAT32、NTFS、EXT2、EXT3、EXT4等），在磁盘的特定区域写入特定数据，在分区中划出一片用于存放文件分配表、目录表等用于文件管理的磁盘空间。

- 硬件设备文件名

![硬件设备文件名](http://img.blog.csdn.net/20160417151024309)

设备文件名：
/dev/hda1 (IDE 硬盘接口)
/dev/sda1 (SCSI 硬盘接口、SATA 硬盘接口)

a: 第一个硬盘
1: 第一个分区

文件系统结构：
![文件系统结构](http://img.blog.csdn.net/20160417161212847)

##3. 文件权限

Linux 系统中每一个文件都有访问权限，在这里记录着哪个用户可以以何种方式访问这个文件，或者禁止访问当前这个文件。

文件权限记录分为两个部分，一部分记录文件的从属关系，一部分记录的是文件的访问方式。例如当使用 `ls -al` 查看文件时：
![文件权限](http://img.blog.csdn.net/20160429102429643)

- 第一部分
即第一个字符，它记录文件类型。常见类型如下：

```
'd' - 目录
'-' - 文件
'l' - 软链接 
's' - socket  
'p' - named pipe   
'b' - block device   
'c' - character device
```
- 第二部分
剩余的字符记录了文件的权限，文件权限每三个字符一段共分为三段

```
第 1 段：所有者的权限 (u)
第 2 段：所属组的权限 (g)
第 3 段：其他用户的权限 (o)
```
用户组：相同身份或相同权限

文件权限每一段有三位，对应分别为 可读/可写/可执行

```
r        “可读” 用数字 4 表示
w        “可写” 用数字 2 表示
x (小写)  “可执行” 用数字 1 表示    
-         “无权限” 用数字 0 表示    
X (大写)  只有目标文件对某些用户是可执行的或该目标文件是目录时才追加 x 属性。
```
".":  CentOS 6 以后出现，代表 ACL 权限.

##4. 一些简单命令
命令提示符：

```
[root@localhost ~]#

其中：
	root	当前登录用户
	localhost	主机名
	~	当前所在目录（家目录）
	#	超级用户的提示符
		普通用户的提示符是$
```

命令格式：

```
命令 [选项] [参数]

注意：个别命令使用不遵循此格式，当有多个选项时，可以写在一起简化选项与完整选项
	-a 等于 --all
```
PS: 方括号里面内容为可选项。

###4.2 目录的相关操作
- 查询目录中的内容：ls

```
ls [选项] [文件或目录]
选项：
	-a	显示所有文件，包括隐藏文件
	-l	显示详细信息
	-d	查看目录属性
	-h	人性化显示文件大小
	-i	显示inode
```

PS: ls -l 可简写为 ll

- 新建目录：mkdir

```
mkdir -p [目录名]
-p	递归创建
命令英文愿意：make directories
```

-p 的作用，例如：

```
mkdir -p test/hello
```
 test 目录不存在时，想要创建 hello 目录必须加 -p

- 切换目录：cd

```
cd [目录]
命令英文原意：change directory

简化操作：
cd ~
cd		进入当前用户的家目录

cd -	进入上次目录
cd ..	进入上级目录
cd .	进入当前目录
```

- 删除空目录：rmdir 

```
rmdir [目录名]
命令英文原意：remove empty directories
```
PS: 该命令用的较少。

- 删除文件或目录：rm

```
rm -rf [文件或目录]
命令英文原意：remove
选项：
	-r	删除目录
	-f	强制
```

自杀命令（删除根目录）：

```
rm -rf /
```

- 复制目录：cp

```
cp [选项] [原文件或目录] [目标目录]
命令英文原意：copy

选项：
-r	复制目录
-p	连带文件属性复制
-d	若源文件是链接文件，则复制链接属性
-a	相当于 -pdr
```

若目标文件只是一个目录，没有文件名，则是原名复制；
若目标文件后面有文件名，则为改名复制。例如：

```
cp 1.txt tmp/	// 复制到 tmp 目录下，文件名仍为 1.txt
cp 1.txt tmp/2.txt	// 复制到 tmp 目录下，文件名为 2.txt
```

- 剪切或改名命令：mv

``` bash
mv [源文件或目录] [目标目录]
目录英文原意： move	// 类似 cp
```
PS: 若源文件和目标文件在同一个目录下，则是改名。


>[Linux达人养成计划 I](http://www.imooc.com/learn/175)  
>参考：
>[linux文件权限总结](http://www.jianshu.com/p/870c457c7cf7)