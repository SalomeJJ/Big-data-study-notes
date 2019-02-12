[TOC]

# 大数据学习笔记

## 第一阶段：linux和高并发

### linux 操作系统

------



#### linux 虚拟机安装

安装centos精简版，虚拟机名称为big-data01

-------



#### linux网络配置

配置网络，拍摄快照，克隆三个虚拟机，big-data02,big-data03,big-data04

--------



#### linux简单命令

1、**type** 判断命令类型 

* 外部命令&内部命令
* 可以找到命令所在的文件路径，进而用cat命令打开

2、**file** 文件类型   ps：ELF表示二进制文件

3、**shell**是个壳

![](D:\file\learning_materials\大数据\笔记\assets\1548904397574.png)

 ps:如果是外部命令去磁盘找，内部命令直接调用

4、**help** :内部命令帮助

5、**man**：帮助手册manual

6、安装yum

​	`yum install man -y` 

7、看到不认识的命令，先判断其是外部还是内部命令，如`type -a ls` ，然后 `file 命令路径`  

​      然后 `man ls`  或 `help 命令`

8、**whereis** 定位命令位置

9、**echo** 打印到标准输出

10、**$PATH** 环境变量路径

11、**$LANG** 语言

12、**ps -fe** 进程列表

13、定义数组

```shell
a=（1 2 3）
echo $b   #输出 1
echo ${b[1]} #输出 2
```

14、**echo $$** 输出当前shell的pid（进程号）

15、shell：对于命令查找的方式

* 在PATH记录的目录中查找

* 缓存到内存hash中
* hash -r 清除缓存

16、

* man

  –1：用户命令(/bin, /usr/bin, /usr/local/bin)

  –2：系统调用

  –3：库用户

  –4：特殊文件(设备文件)

  –5：文件格式(配置文件的语法)

  –6：游戏

  –7：杂项(Miscellaneous)

  –8: 管理命令(/sbin, /usr/sbin, /usr/local/sbin)

--------



 #### 文件系统

- Filesystem Hierarchy Standard（文件系统层次化标准）

  * /boot: 系统启动相关的文件，如内核、initrd，以及grub(bootloader)
  * /dev: 设备文件  
  * /etc：配置文件
  * /home：用户的家目录，每一个用户的家目录通常默认为/home/USERNAME
  * /root：管理员的家目录；
  * /lib：库文件
  * /media：挂载点目录，移动设备
  * /mnt：挂载点目录，额外的临时文件系统
  * /opt：可选目录，第三方程序的安装目录
  * /proc：伪文件系统，内核映射文件
  * /sys：伪文件系统，跟硬件设备相关的属性映射文件
  * /tmp：临时文件, /var/tmp
  * /var：可变化的文件
  * /bin: 可执行文件, 用户命令
  * /sbin：管理命令



----------

### linux基本命令

----------

#### linux文件系统命令

- df：显示磁盘使用情况
- du：显示文件系统使用情况
- ls：显示目录
- cd：切换工作目录
- pwd：显示当前工作目录
- mkdir：创建目录
- rm：删除
- cp：拷贝
- mv：移动
- ln：链接
- stat：元数据
- touch

**注意点**

* ls可以查看多个路径 ，如 `ls / /etc` 

  多个目录的显示顺序 同级按首字母排  不同级则靠近根的排前面

* **ls -l**

  * 文件类型：

    * -：普通文件 (f)

    * d: 目录文件

    

    * b: 块设备文件 (block)

    * c: 字符设备文件 (character)

    

    * l: 符号链接文件(symbolic link file)

    

    * p: 命令管道文件(pipe)

    * s: 套接字文件(socket)

  * 文件权限：9位，每3位一组，3组 权限（U,G,O）每一组：rwx(读，写，执行), r--

  * 文件硬链接的次数

  * 文件的属主(owner)

  * 文件的属组(group)

  * 文件大小(size)，单位是字节

  * 时间戳(timestamp)：最近一次被修改的时间

    * 访问:access

    * 修改:modify，文件内容发生了改变

    * 改变:change，metadata，元数据	



* 直接 **cd** 是到当前用户的家目录

* ```shell
  mkdir x/adir x/bdir x/cdir
  mkdir x/{a,b,c}dir#{}拓展
  #上面两个命令等效
  mkdir x/{a,b,c}#这样也可以
  ```

* **mv**有两个用途，1、移动文件目录   2、重命名

* **in** 注意软链接和硬链接的区别

* **touch** 有两个用途 1、当文件已存在时刷新访问时间 2、当文件不存在时，创建文件

---------



#### 文本操作命令

* **cat** 缺点：文件可能太长 解决办法 ：用 **more**指令

* **less** 类似more，但可以按**B**回滚

* **head**  Print  the  first  10  lines  of each FILE to standard output.

  `head -整数a 文件名   #查看文件前a行内容`

* **tail** output the last part of file  相对于head      

  **-f** ：output appended data as the file grows 起到监控作用

* **管道** 

  `cat demo1.txt | head -3`

  把左边的输出作为右边的输入

  * shell读取用户输入的字符串

  * 发现 |，代表有管道

  * | 左右被理解为简单命令

  * 加工：前一个（左边）简单命令的标准输出

  * 指向后一个（右边）简单命令的标准输入

  * 注意：后一个简单命令一定能够接受标准输入

    

  **xargs** build and execute command lines from standard input 从标准输入构建和执行命令行

  `echo "/" | xargs ls -l `

  `head -5 filename | tail -1`查看某个文件的第五行

-------------

#### vi全屏编辑器1

* **打开文件**

  * vim /path/to/somefile

  * vim +# :打开文件，并定位于第#行 

  * vim +：打开文件，定位至最后一行

  * vim +/PATTERN : 打开文件，定位至第一次被PATTERN匹配到的行的行首

* **关闭文件**

  末行模式：

  * :q  退出  没有动过文件

  * :wq 保存并退出   动过了，不后悔

  * :q! 不保存并退出  动过了，后悔了

  * :w 保存

  * :w! 强行保存

  * :wq --> :x

  * –ZZ: 保存并退出   不需要冒号，编辑模式

    

* 模式：

  –**编辑模式**：按键具有编辑文本功能：默认打开进入编辑模式

  –**输入模式**：按键本身意义

  –**末行模式**：接受用户命令输入

  

**模式转换**

* **编辑-->输入：**

  –  i: 在当前光标所在字符的前面，转为输入模式；

  –  a: 在当前光标所在字符的后面，转为输入模式；

  – o: 在当前光标所在行的下方，新建一行，并转为输入模式；

  –  O：在当前光标所在行的上方，新建一行，并转为输入模式；  

  –  I：在当前光标所在行的行首，转换为输入模式

  –  A：在当前光标所在行的行尾，转换为输入模式

  –  输入-->编辑：

  * ESC

* **编辑-->末行：**

  –：

* **末行-->编辑：**

  –ESC, ESC

-------------------------------

#### vi全屏编辑器2

**编辑模式**

* **移动光标**

  –**字符**

  - h: 左；j: 下；k: 上；l: 右

  –**单词**

  * **w**: 移至下一个单词的词首

  * **e**: 跳至当前或下一个单词的词尾

  * **b**: 跳至当前或前一个单词的词首

  –**行内**

  * **0**: 绝对行首

  * **^**: 行首的第一个非空白字符

  * **$**: 绝对行尾

  –**行间**

  * **G**:文章末尾

  * **3G**:第3行

  * **gg**:文章开头

  –**翻屏**

  * ctrl：f，b

* **删除&替换单个字符**

  * **x**:删除光标位置字符

  * **3x**:删除光标开始3个字符

  * **r**:替换光标位置字符

* **删除命令 ： d** 

  * **dw**（删除单词）5dw 删除多个单词

  * **dd**（删除一行）5dd 删除五行

* **复制粘贴&剪切**  

  * **yw**（复制一个单词） 3yw 复制3个单词
  * **yy**（复制一整行） 

  * **p** 粘贴

  * **dd**还有剪切功能

* **撤销&重做**

  * u   撤销

  * ctrl+r  重做 撤销的操作

  * .  重复上一步的操作



**末行模式   shift+：**

* **set**：设置

  * **set nu**  number

  * **set nonu** nonumber

  * **set readonly** 只读
  * **set noreadonly** 撤销只读

* **/**：查找

  * /after（*直接/和：/不一样*）

  * n，N（n向下，N向上）

  * ？向上查找

* **！**：执行命令

  * :! ls -l/

* **s**查找并替换

  * **s/str1/str2/gi**

    * /：临近s命令的第一个字符为边界字符：/，@，# （因为有时候我们查找的内容里面可能包含/）

    * g：一行内全部替换（不加g的话，只会把该行匹配到的第一个替换，而其他的不替换）

    * i：忽略大小写

  * 范围（必须有范围）

    * n：行号

    * .：当前光标行

    * +n：偏移n行

    * \$：末尾行，​$​-3

    *  %：全文

* 删除全文

  `:.,$d`

  `dG`

* 从第一行删到倒数第二行

  `1,$-2d`

* 复制第三行到第九行

  `3，9y`

--------------------------

#### 正则表达式

* **grep**：显示匹配行

  * –v：反显示

  * –e：使用扩展正则表达式

**正则表达式**   *REGULAR EXPRESSIONS*

* **匹配操作符**

  * \                  转义字符

  * .                    匹配任意**单个**字符

  * [1249a]（包含这5个字符里的任意字符都能显示出来）[\^12]（非1,2以外的其他字符）,[a-k] （a-k任意字符）    字符序列单字符占位

  * ^                 行首

  * $                  行尾

  * \\<,\\>：\\<abc           单词首尾边界

  * ==|           连接操作符==

  * ==(,)          选择操作符==

  * ==\n         反向引用==

* **重复操作符:**

  * ?        匹配0到1次。

  * \* 匹配0到多次。

  + \+ 匹配1到多次。

  * {n}     匹配n次。

  * {n,}    匹配n到多次。

  * {n,m}      匹配n到m次。

* 与**扩展正则表达式**的区别:grep basic

  ​    \?, \\+, \\{, \\|, \\(, and \\)

* 匹配任意字符

  **.***



例

* `grep "ooxx" grep.txt`  显示grep.txt里包含ooxx的行

  ![1549006112593](D:\file\learning_materials\大数据\笔记\assets\1549006112593.png)

* `grep "[0-9]" grep.txt` 显示包含数字的行

  ![1549006272860](D:\file\learning_materials\大数据\笔记\assets\1549006272860.png)

  

* `grep "[34]" grep.txt` 显示包含数字3 **或** 4的行   （单字符占位）

  ![1549006460537](D:\file\learning_materials\大数据\笔记\assets\1549006460537.png)

* `grep "[0-9]\{4\}" grep.txt` 显示包含四位整数的行 注意 {}是拓展正则表达式，要加 \

  ![1549011488982](D:\file\learning_materials\大数据\笔记\assets\1549011488982.png)

* `grep -E "[0-9]{4}" grep.txt` 和上面一样的效果，不需要加 \

* `grep "\<ooxx\>" grep.txt` 包含单词ooxx的行

  ![1549011984713](D:\file\learning_materials\大数据\笔记\assets\1549011984713.png)

* `grep "[^0-9][0-9]\{4\}[^0-9]" grep.txt` 只包含四位数字的行（不能多）

  ​	![1549012404502](D:\file\learning_materials\大数据\笔记\assets\1549012404502.png)

* `grep -E "(^[0-9]|[^0-9][0-9])[0-9]{2}([0-9][^0-9]|[0-9]$)"  grep.txt`

  与上面比较

  ![1549073410683](D:\file\learning_materials\大数据\笔记\assets\1549073410683.png)

* `grep -E "(^[0-9]{4}[^0-9]|[^0-9][0-9]{4}[^0-9]|[^0-9][0-9]{4}$|^[0-9]{4}$" grep.txt`

  与上一种进行比较

  ![1549073180769](D:\file\learning_materials\大数据\笔记\assets\1549073180769.png)

* `grep "\<aaa" test`

  `grep "\<aaa\>" test`

  ![1549075781023](D:\file\learning_materials\大数据\笔记\assets\1549075781023.png)

* `grep -E ".*(god).*(good).*\1.*\2" test`

  `grep -E ".*(god).*(good).*\2.*\1" test`

  含义：第一个括号就是\1  第二个括号就是\2  **反向引用** 

  ![1549076372876](D:\file\learning_materials\大数据\笔记\assets\1549076372876.png)

----------------------------------------

#### 文本分析-cut-sort-wc

**cut**：显示切割的行数据

​	–f：选择显示的列

​	–s：不显示没有分隔符的行

​	–d：自定义分隔符



​	`cut -d" " -f1  grep.txt`  按空格分割

​	![1549077588898](D:\file\learning_materials\大数据\笔记\assets\1549077588898.png)

​	

​	`cut -d" " -f1,2 grep.txt`

​	![1549077765907](D:\file\learning_materials\大数据\笔记\assets\1549077765907.png)

​	

​	`cut -d" " -s -f1-3 grep.txt`  -s不显示没有分隔符的行

​	![1549077929949](D:\file\learning_materials\大数据\笔记\assets\1549077929949.png)

​	

​	`cut -d" " -s -f3 grep.txt`

​	![1549078087786](D:\file\learning_materials\大数据\笔记\assets\1549078087786.png)

​	

​	`cut -d':' -f1 passwd` 取出第一列

​	![1549082891018](D:\file\learning_materials\大数据\笔记\assets\1549082891018.png)

​		

**sort**：排序文件的行

​	ps：排序有两种 ：1、按字典序排 2、按数值序排

​	如：1，2，3，11，12，21，22（按数值序排）

​		1，11，12，2，21，22（按字典序排）



​	–n：按数值排序

​	–r：倒序

​	–t：自定义分隔符

​	–k：选择排序列

​	–u：合并相同行

​	–f：忽略大小写



​	`sort sort.txt `   按字典序排（每一行第一个字母）

​	![1549083556335](D:\file\learning_materials\大数据\笔记\assets\1549083556335.png)

​	

​	`sort -t" " -k2 -n sort.txt` 按后面的数值大小进行排序

​	![1549083773961](D:\file\learning_materials\大数据\笔记\assets\1549083773961.png)



​	`sort -t" " -k2 -n -r sort.txt` 按数值逆序

​	![1549111183059](D:\file\learning_materials\大数据\笔记\assets\1549111183059.png)

​	

​	`sort -t" " -n -k1 sort.txt` 如果把第一列非数字按数值排序.......

​	![1549111655959](D:\file\learning_materials\大数据\笔记\assets\1549111655959.png)

​	

**wc**: word count 统计单词

​	功能：print newline, word, and byte counts for each file  行数 单词数 字节数（空格and每行的$也算）

​	![1549111815354](D:\file\learning_materials\大数据\笔记\assets\1549111815354.png)

​	![1549112028416](D:\file\learning_materials\大数据\笔记\assets\1549112028416.png)



​	**选项：**

```
	   -c, --bytes
              print the byte counts

       -m, --chars
              print the character counts

       -l, --lines
              print the newline counts
              
       -w, --words
              print the word counts
```

​	小技巧：

​	wc sort.txt  输出结果后面会跟sort.txt （文件名称）  解决办法  `cat sort.txt|wc` 

------------------------------------

#### 文本分析-sed1

sed：行编辑器

**sed** [**options**] '**Address Command**' file ...

* **options**

​	-n: 静默模式，不再默认显示模式空间中的内容

​	-i: 直接修改原文件

  	-e SCRIPT -e SCRIPT:可以同时执行多个脚本

​	-f /PATH/TO/SED_SCRIPT

​	-r: 表示使用扩展正则表达式

* **Command**

  * d: 删除符合条件的行；

  * p: 显示符合条件的行；

  * a \string: 在指定的行后面追加新行，内容为string

  * \n：可以用于换行

  * i \string: 在指定的行前面添加新行，内容为string

  * r FILE: 将指定的文件的内容添加至符合条件的行处

  * w FILE: 将地址指定的范围内的行另存至指定的文件中; 

  * s/pattern/string/修饰符: 查找并替换，默认只替换每行中第一次被模式匹配到的字符串
    * g: 行内全局替换
    * i: 忽略字符大小写
    * s///: s###, s@@@  
    * \\(\\), \1, \2

* **Address**

  * 可以没有

  * 给定范围

  * 查找指定行/str/



操作实例

`sed '2p' sort.txt` 显示第二行

`sed -n '2p' sort.txt` -n 静默模式，不再默认显示模式空间中的内容

![1549173033929](D:\file\learning_materials\大数据\笔记\assets\1549173033929.png)

`sed '3d' sort.txt` 删除第三行（源文件不变）

`sed -i '3d' sort.txt` 删除第三行（改变源文件）

![1549173700745](D:\file\learning_materials\大数据\笔记\assets\1549173700745.png)

`sed '2i\jiyuan' sort.txt `  在第二行前面添加新行（jiyuan）

`sed '2a\jiyuan' sort.txt `  在第二行后面添加新行（jiyuan）

同样，以上两种均不改变源文件，加 **-i** 改变

![1549174068338](D:\file\learning_materials\大数据\笔记\assets\1549174068338.png)

![1549174192930](D:\file\learning_materials\大数据\笔记\assets\1549174192930.png)

`sed "s/jiyuan/salome/" sort.txt ` 查找替换

![1549174541354](D:\file\learning_materials\大数据\笔记\assets\1549174541354.png)

`sed '/jiyuan/d' sort.txt `  删除符合条件的指定行 （包含"jiyuan"的行）

![1549174750527](D:\file\learning_materials\大数据\笔记\assets\1549174750527.png)



`sed 's/\(id:\)3\(:initdefault:\)/\14\2/' inittab`   把3改成4（用到了正则里的反向引用）

![1549176752788](D:\file\learning_materials\大数据\笔记\assets\1549176752788.png) 

![1549176771395](D:\file\learning_materials\大数据\笔记\assets\1549176771395.png)



----------------

#### 文本分析-sed2

处理ip地址

`sed "s/\(IPADDR=\(\<2[0-5][0-5]\|\<2[0-4][0-9]\|\<1\?[0-9][0-9]\?\.\)\{3\}\).*/\188/"
ifcfg-eth0`

`sed "s/\(IPADDR=\(\([0-9]\|[1-9][0-9]\|1[0-9][0-9]\|2[0-4][0-9]\|25[0-5]\)\.\)\{3\}\).*/\190/" ifcfg-eth0`  

![1549179803344](D:\file\learning_materials\大数据\笔记\assets\1549179803344.png)

 

--------------------

#### 文本分析-awk1

**awk**

* awk是一个强大的文本分析工具。

* 相对于grep的查找，sed的编辑，awk在其对数据分析并生成报告时，显得尤为强大。

* 简单来说awk就是把文件逐行的读入，（空格，制表符）为默认分隔符将每行切片，切开的部分再进行各种分析处理。



**awk -F '{pattern + action}' {filenames}**    pattern:模式匹配   action:动作

* 支持自定义分隔符（默认是空格或制表符）

* 支持正则表达式匹配

* 支持自定义变量，数组  a[1]  a[tom]  map(key)

* 支持内置变量

  * ARGC               命令行参数个数

  * ARGV               命令行参数排列

  * ENVIRON            支持队列中系统环境变量的使用

  * FILENAME           awk浏览的文件名

  * FNR                浏览文件的记录数

  * FS                 设置输入域分隔符，等价于命令行 -F选项

  * **NF                 浏览记录的域的个数 （分隔符分好后每行的列数）**

  * **NR                 已读的记录数**

  * OFS                输出域分隔符

  * ORS                输出记录分隔符

  * RS                 控制记录分隔符

* 支持函数
  * print、split、substr、sub、gsub

* 支持流程控制语句，类C语言
  * if、while、do/while、for、break、continue



操作

* `cat passwd` 打开passwd文件

  ![1549682997093](D:\file\learning_materials\大数据\笔记\assets\1549682997093.png)

  补充：每一行的列的意义  

  用户名称，密码，属主，属组，用户名称描述，家目录，对应的shell程序

  命令 id 用户名查看该用户相关信息 如 id root

* 需求1：只是显示/etc/passwd的账户:CUT     （第一列）

  `awk -F ':' '{ print $1}' passwd` 打印第一列   ==awk必须要用单引号== 

  `cut -d ":" -f1 passwd ` the same  effect

  ![1549702579593](D:\file\learning_materials\大数据\笔记\assets\1549702579593.png)

* 只是显示/etc/passwd的账户和账户对应的shell,而账户与shell之间以制表符分割,而且在所有行开始前添加列名name,shell,在最后一行添加"blue,/bin/nosh"（cut，sed）

  `awk -F':' 'BEGIN{print "name\tshell"} {print $1 "\t" $7} END{print "blue\t/bin/nosh"}'
  passwd` BEGIN和END是自定义函数

  ![1549703152468](D:\file\learning_materials\大数据\笔记\assets\1549703152468.png)

* 搜索/etc/passwd有root关键字的所有行

  `awk '/root/ { print $0}' passwd` $0表示这一行的所有内容

  ![1549703407961](D:\file\learning_materials\大数据\笔记\assets\1549703407961.png)

* 统计/etc/passwd文件中，每行的行号，每行的列数，对应的完整行内容

  `awk -F ':' '{print NR "\t" NF "\t" $0}' passwd` 

  ![1549704291419](D:\file\learning_materials\大数据\笔记\assets\1549704291419.png)

  

------------

#### 文本分析-awk2

![1549957716801](D:\file\learning_materials\大数据\笔记\assets\1549957716801.png)

统计报表，合成每人一月工资  0：manager，1：worker

```shell
awk '{
split($3,date,"-");  #按"-"分割第三列元素，存到数组date中
if(date[2]=="01"){
name[$1]+=$5
};
}
END{
for(i in name){
print i "\t" name[i]"\t"
}
}' awk.txt
```

![1549958197824](D:\file\learning_materials\大数据\笔记\assets\1549958197824.png)

显示worker manger

```shell
awk '{
split($3,date,"-");
if(date[2]=="01"){
name[$1]+=$5
};
if($2=="0"){
role[$1]="manger"
}
else{
role[$1]="worker"
}
}
END{
for(i in name){
print i "\t" name[i]"\t" role[$1]
}
}' awk.txt
```

![1549958513446](D:\file\learning_materials\大数据\笔记\assets\1549958513446.png)

把命令保存为.sh文件

```shell
#awk.sh
{
	split($3,date,"-");
	if(date[2]=="01"){
		name[$1]+=$5
	};
	if($2=="0"){
		role[$1]="manger"}
	else{
		role[$1]="worker"
	}
}
END{
	for(i in name){
		print i "\t" role[i] "\t" name[$1]
	}
}
```

```shell
awk -f awk.sh awk.txt

:'
awk -f 参数
-f program-file
       --file program-file
              Read  the  AWK program source from the file program-file, instead of from the first command line argument.  Multiple -f (or --file) options
              may be used.
'
```


