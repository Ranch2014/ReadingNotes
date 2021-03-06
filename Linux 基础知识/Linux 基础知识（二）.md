##1. 常用目录
常用的目录如下所示：

``` bash
/		根目录
/bin	命令保存目录（普通用户可以读取的命令）
/boot	启动目录，启动相关文件
/dev	设备文件保存目录
/etc	配置文件保存目录
/home	普通用户的家目录
/lib	系统库保存目录
/mnt	系统挂载目录
/media	挂载目录
/root	超级用户家目录
/tmp	临时目录
/sbin	命令保存目录（超级用户才能使用的命令）
/proc	直接写入内存的
/sys	
/usr	系统软件资源目录
	/usr/bin	系统命令（普通用户）
	/usr/sbin	系统命令（超级用户）
/var	系统相关文档
```
PS:   
	1. proc 和 sys 目录不能直接操作（这两个目录保存的是内存的挂载点）。  
	2. 根目录下的 bin  和 sbin，usr 目录下的 bin 和 sbin，这四个目录都是用来保存系统命令的。  
	3. 可以在家目录 root 或 home，以及 tmp 目录下随意放内容。

##2. 链接命令
```bash
ln -s [源文件] [目标文件]

命令英文原意：link
功能描述：生成链接文件
	选项：-s 创建软链接
```

- 硬链接 (不推荐) 

特性：  
1. 拥有相同的 iNode 和存储 block，可以看做是同一个文件  
2. 可以通过 iNode 识别  
3. 不能跨分区  
4. 不能针对目录使用  

- 软链接 (推荐)

特性：  
1. 类似 Windows 快捷方式  
2. 软链接有自己的 iNode 和 block，但数据中保存的是（原）文件名和 iNode，并没有实际的文件数据  
3. lrwxrwxrwx    l 软链接，权限都是 rwxrwxrwx，但实际权限由原文件的权限决定  
4. 修改任意文件，另一个都改变  
5. 删除原文件，软链接不可用  

##3. 文件搜索命令
- locate

``` bash
locate [文件名]

/var/lib/mlocate  #locate 命令所搜索的数据库后台

updatedb	#强制更新数据库 (默认一天更新一次)
```
优点：速度快。
缺点：只能按文件名搜索。

- whereis
搜索系统命令所在位置（只能查看系统命令）

```bash
whereis 命令名
	# 搜索命令所在路径及帮助文档的位置

选项：
	-b: 只查找可执行文件
	-m: 只查找帮助文件
```

- which

可以看到文件的别名

```bash
which 文件名
	# 搜索命令所在路径及别名
```
二者区分举例：
![whereis 和 which](http://img.blog.csdn.net/20160427202651254)

- 其他  
PATH 环境变量: 定义的是系统搜索命令的路径。

- find  

``` bash
find [搜索范围] [搜索条件]
# 搜索文件，应尽量避免大范围搜索 (非常耗费系统资源)
# find 是完全匹配，若要模糊查询，则需使用通配符。

通配符：
*	匹配任意内容
?	匹配任意一个字符
[]	匹配任意一个括号内的字符
```
一些其他命令：

``` bash
find /root -iname test	#不区分大小写
find /root -user root	#按所有者搜索
find /root -nouser	#查找没有所有者的文件

find /root -mtime +10	#查找10天前修改的文件
-10：10天以内
10：10天当天
+10：10天以前

find . -size 25k	#查找文件的大小为 25K 的文件，点 . 表示当前目录
-25k：小于25K
25k：等于25K
+25k：大于25K
# M (兆) 同理。注意：k 是小写，M 是大写。

find . -size +20k -a -size -50k
# 查找当前目录下，大小在20K~50K之间的文件。
-a: and, 两个都满足
-o: or, 满足一个即可

find . -size +20k -a -size -50k -exec ls -lh {} \;
# 查找当前目录下，大小在20K~50K之间的文件，且显示详细信息。
# -exec *** {} \;  对搜索结果执行的操作
```

- grep
在文件中搜索字符串的命令。

``` bash
grep [选项] 字符串 文件名	#在文件中匹配符合条件的字符串

选项：
	-i: 忽略大小写
	-v: 排除指定字符
```


>[Linux达人养成计划 I](http://www.imooc.com/learn/175)