---
title: "C语言指针与内存"
subtitle: 'C语言指针与内存'
author: "Kevin"
header-style: text
tags:
  - 指针
  - 内存
---

## 7.2 输入流输出流以及错误流的重定向

标准输入流  **➜**  键盘输入

标准输出流  **➜**  终端输出

标准错误流  **➜**  错误输出



## 8.1 管理原理及应用

```bash
➜  les5 ls /etc/ | grep ab
fstab.hd
gettytab
krb5.keytab
rmtab
xtab
```

`grep` 管道符，查找 `/etc` 下所有带有 `ab` 关键词的文件或文件夹

`ls` 和 `grep` 组合在一起可以查看带有关键词的文件或文件夹

#### 查看所有进程里有没有包含 `ssh` 的进程

`ps -e | grep ssh`

```
➜  les5 ps -e | grep ssh
 1826 ??         0:00.09 /usr/bin/ssh-agent -l
10344 ttys000    0:00.00 grep --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn ssh
```



## 9.1 打造实用C语言小程序

`avg.c`

```c
#include <stdio.h>
  
int main()
{
    int s,n;
    scanf("%d,%d",&s,&n);
    float v=s/n;
    printf("v=%f",v);
    return 0;
}
```

`input.c`

##### 工资求和

```c
#include <stdio.h>

int main()
{
    int flag=1;
    int i;
    int count=0;
    int s=0;
    while(flag){
        scanf("%d",&i);
        if(0==i) break;
        count++;
        s+=i;
    }
    printf("%d,%d",s,count);
    return 0;
}
```

```bash
➜  les6 cc input.c -o input.out
➜  les6 ls
avg.c     avg.out   input.c   input.out
➜  les6 ./input.out
3000
2800
5000
9000
4500
800
0
25100,6% //一共工资发了25100，一共发了6个人
```

```bash
➜  les6 rm input.out
➜  les6 cc input.c -o input.out
➜  les6 ls
avg.c     avg.out   input.c   input.out
➜  les6 ./input.out
3000
2000
0
5000,2% //工资一共发了5000，发给了2个人
```

`avg.out` 

##### 求平均值

```bash
➜  les6 ./avg.out
100,2
v=50.000000%  
```

##### 将两个程序结合在一起算工资平均数

`./input.out | ./avg.out`

```bash
➜  les6 ./input.out | ./avg.out
800
3000
4500
8000
12000
7500
15000
20000
0
v=8850.000000% 
```



## 2.1 Linux C语言 初始指针

```c
#include <stdio.h>
  
void change(int *a,int *b)
{
    int tmp=*a;
    *a=*b;
    *b=tmp;
}

int main()
{
    int a=5;
    int b=3;
    change(&a,&b);

    printf("num a=%d\nnum b=%d\n",a,b);
}
```

```bash
➜  pointer gcc main.c -o main.out 
➜  pointer ./main.out
num a=3
num b=5
```

> 这是实验说明我们把change函数改造之后就能成功交换了，如果不改造，不加`*`和`&`会不生效

`&` ➜ 取地址符



## 3.1 gdb工具的使用

`brew install gdb` 安装 `gdb` 工具

`gdb -help` 能完整显示里面的用法

让`main2.c` 文件出错，将`*`删除

```c
#include <stdio.h>
  
void change(int a,int b)
{
    int tmp=*a;
    a=b;
    b=tmp;
}

int main()
{
    int a=5;
    int b=3;
    change(a,b);

    printf("num a=%d\nnum b=%d\n",a,b);
}
```

然后`gcc -g main2.c -o main2.out ` 

再然后 `gdb ./main2.out` 进行调试

```bash
(gdb) start
(gdb) p a
$1 = 0
(gdb) n
13		int b=3;
(gdb) p a
$2 = 5
```

退出`gdb` ,`quit` 命令

`gdb main.out`



## 3.2 使用gdb调试案例

- 什么是堆内存
- 什么是栈内存
- 内存地址
- 指针变量的实质是什么东西



## 4.1 计算机中的数据表示方法

单位：字节（Byte）

1Byte = 8bit

高电位：1

低电位：0

二进制：满二进一 `11 + 1 = 100`

十六进制：0x开头`0` `1` ...`7` `0x8` `9` `A` `B` `C` `D` `E` `0xF` `10`

##### 扩展

`char`

字符在计算机的存储器中以字符编码的形式保存，字符编码是一个数字，因此在计算机看 来，A与数字65完全一样。（65是A的ASCII码）

`int`

如果你要保存一个整数，通常可以使用int。不同计算机中int的大小不同，但至少应该有16位。一般而言，int可以保存几万以内的数字。

`short`

但有时你想节省一点空间，毕竟如果只想保存一个几百、几千的数字，何必用int?可以用short，short通常只有int的一半大小。

`long`

但如果想保存一个很大的计数呢?long数据类型就是为此而生的。在某些计算机中，long的 大小是int的两倍，所以可以保存几十亿以内的数字;但大部分计算机的long和int一样大， 因为在这些计算机中int本身就很大。long至少应该有32位。

`float`

float是保存浮点数的基本数据类型。平时你会碰到很多浮点数，比如一杯香橙摩卡冰乐有多 少毫升，就可以用float保存。

`double`

但如果想表示很精确的浮点数呢?如果想让计算结果精确到小数点以后很多位，可以使用double。double比float多占一倍空间，可以保存更大、更精确的数字。



## 4.2 内存管理

内存由操作系统统一管理

1个字节有8个二进制位

插两根2G内存 <=> 插一根4G内存条

把内存看成一个整体来计算内存大小

`32bit` 操作系统最大使用`4G`内存

为什么32位操作系统只能使用4G内存呢？

> 地址总线是32位，也就是寻址空间是32位

32位指的是：

> 给内存编号只能编到32个二进制位

这个编号就类似我们街道的门牌号码一样

![image-20190924112904123](assets/image-20190924112904123.png)

![image-20190924112831658](assets/image-20190924112831658.png)

32根地址总线就有2^32个状态

2的32次方个字节等于多少呢？

`2^10*2^10*2^10*2^2`

= `1024*1024*1024*4`字节

= `1024 * 4M`

= `4G`

64bit为主机 `2^64`

操作系统会对所有内存进行编号

编号 = 唯一的内存字节的地址



##### 64位操作系统内存地址编号

二进制表示：0000 0000 ... ... 0000 0000



#### 用户内存隔离开的好处

-操作系统的内存不会被大量占用

-避免机器卡住，卡死，死机等状态

-可通过操作系统把应用程序关闭

-使得操作系统更加安全



![image-20190924114458384](assets/image-20190924114458384.png)

C语言代码

-求面积

-计算平均数

函数编译之后存到磁盘

声明一些全局变量或者声明一些常量

`const int i`

![image-20190924114720854](assets/image-20190924114720854.png)

##### C语言中所有的变量都有类型

`int` 类型就保存整数

`double` 类型就保存的双精度的浮点数

##### 那么指针保存的是什么呢？

指针保存的就是内存地址

![image-20190924153517004](assets/image-20190924153517004.png)

每个函数都是在`栈内存`中记录信息的

- 数据段
    - 全局变量
    - 常量

一个函数可以被多次调用



## 4.5 函数栈以及数据段内存

##### 栈

> 这是存储器用来保存局部变量的部分。每当调用函数，函数的所有局部变量都在栈 上创建。它之所以叫栈是因为它看起来就像 堆积而成的栈板:当进入函数时，变量会 放到栈顶;离开函数时，把变量从栈顶拿 走。奇怪的是，栈做起事来颠三倒四，它从存储器的顶部开始，向下增长。

##### 堆

> 我们还没有用过这部分的存储器，堆用于`动态存储`:程序在运行时创建一些数据， 然后使用很长一段时间，稍后会看到堆的 用法。

##### 全局量

> 全局量位于所有函数之外，并对所有函数 可见。程序一开始运行时就会创建全局量，你可以修改它们，不像......

##### 常量

> 常量也在程序一开始运行时创建，但它们 保存在只读存储器中。常量是一些在程序 中要用到的不变量，你不会想修改它们的 值，例如字符串字面值。

##### 代码

> 最后是代码段，很多操作系统都把代码放 在存储器地址的低位。代码段也是只读的， 它是存储器中用来加载机器代码的部分。



## 4.6 函数指针与指针指向的数据访问

##### C代码包含指针

> 指针是理解C语言最基本的要素之一。那么什么是指针?指针就是存储器中某条数据的地址。 之所以要在C语言中使用指针有以下几个原因。

- 在函数调用时，可以只传递一个指针，而不用传递整份数据。
- 让两段代码处理同一条数据，而不是处理两份独立的副本。

> 指针做了两件事:避免副本和共享数据。但既然指针只是地址而 已，为什么它会令很多人感到困惑呢?因为指针是一种间接形式 的地址。在茫茫存储器中追逐指针，一不小心就会迷路。而学习C指针的诀窍就是慢慢来。



##### 使用存储器指针

- 得到变量的地址

可以用**&**运算符找到变量保存在存储器中的位置，这你已经知道了:

一旦得到了变量的地址，就需要把它保存在某个地方。为此，需 要指针变量。指针变量是一个用来保存存储器地址的变量。当 声明指针变量时，需要说明指针所指向的地址中保存的数据的类 型:

- 读取地址中的内容

当你有了存储器地址，就想读取保存在那里的数据，这时可以 用*****运算符:

`*`运算符和`&`运算符恰好相反。`&`运算符接收一个数据，然后告诉 你这个数据保存在哪里;*运算符接收一个地址，然后告诉你 这个地址中保存的是什么数据。因为指针有时也叫引用，所以*运算符也可以描述成对指针进行解引用。

- 改变地址中的内容

如果你有一个指针变量，并想修改这个变量所指向地址中的数 据，可以再次使用`*`运算符，只不过这次需要把指针变量放在 赋值运算符的左边。