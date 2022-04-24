# Shell编程

[TOC]



# 一、概述

**Shell** 是一个命令解释器，用户可以用shell来启动、挂起、停止甚至是编写一些程序（也是一个编程语言）。

这个交互界面就是Shell：

![image-20220422153251185](https://gitee.com/jackieling/xp01/raw/master/202204221804316.png)

terminal是输入命令和显示输出的那个窗口，shell是窗口背后在解释你输入的程序。

你在 windows上面打开的cmd和powershell都是窗口，但cmd默认解释输入的命令的方式是cmd，powershell则是powershell，Linux和mac上的terminal打开以后默认用的是shell，有些默认用的是cshell，有些用的是bash，这两个都是shell。


著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

![image-20220422152843744](https://gitee.com/jackieling/xp01/raw/master/202204221804364.png)

shell其实就是Linux里的一个翻译官，负责**把命令翻译成二进制机器语言数字**。

内核拥有最高级别、最底层的权限，在接受到shell命令以后可以直接控制硬件。

![image-20220422155659872](https://gitee.com/jackieling/xp01/raw/master/202204221804366.png)

# 二、shell脚本执行方式

## 2.1编写第一个脚本

```shell
"hello.sh" [New] 5L, 71C written

#先创建sh文件夹，在里面写代码：
[root@localhost ~]# mkdir sh
[root@localhost ~]# ls
anaconda-ks.cfg  fengjie             Japan                sh       模板  下载
cangjin          hello.sh            Japanlovestory.list  yangmi   视频  音乐
cangjinwq        install.log         linghu               zhengyi  图片  桌面
changjin         install.log.syslog  nvshen               公共的   文档
[root@localhost ~]# cd sh/
##写脚本代码hello.sh
[root@localhost sh]# vi hello.sh


##第一行是声明这个是shell脚本，指定解析器，必须有
#!/bin/bash
#the first  program
#author:linghu

echo -e "Hello world!"
~                                                                               
~                                                                               
~                                                                               
~                                                                               
~                                                                               
                                                                             
"hello.sh" 6L, 72C written
[root@localhost sh]# chmod 755 hello.sh
[root@localhost sh]# ll
总用量 4
-rwxr-xr-x. 1 root root 72 4月  22 16:13 hello.sh
[root@localhost sh]# ls
hello.sh

##通过绝对路径执行shell脚本
[root@localhost sh]# /root/sh/hello.sh
Hello world!
[root@localhost sh]# 

##通过bash+脚本文件执行命令
[root@localhost sh]# bash hello.sh
Hello world!
[root@localhost sh]# 
[root@localhost sh]# 
```

用shell脚本写的一个俄罗斯方块游戏：game.sh:

![image-20220422162811755](https://gitee.com/jackieling/xp01/raw/master/202204221804367.png)

## 2.2多命令操作案例

需求：

![image-20220422170842542](https://gitee.com/jackieling/xp01/raw/master/202204221804368.png)



```shell
##先把目录创建好：
[root@localhost sh]# cd 
[root@localhost ~]# cd /home
[root@localhost home]# mkdir atguigu

##开始写脚本代码：
[root@localhost ~]# cd sh
[root@localhost sh]# touch batch.sh
[root@localhost sh]# vim batch.sh
#!/bin/bash

cd /home/atguigu/

touch banzhang.txt

echo "I love cls" >> banzhang.txt
~                                                                                                                                                   
~                                                                                                                                                   
~                                                                                                                                                                                                         
"batch.sh" 7L, 86C 已写入   
[root@localhost sh]# bash batch.sh

##执行完毕开始查看验证脚本是否执行成功：

[root@localhost ~]# cd sh
[root@localhost sh]# bash batch.sh
[root@localhost sh]# cd /home/atguigu
[root@localhost atguigu]# ls
banzhang.txt
[root@localhost atguigu]# vi banzhang.txt
I love cls
~            
```

![image-20220422172023414](https://gitee.com/jackieling/xp01/raw/master/202204221804370.png)

# 三、Shell中的变量

Shell变量分为：

+ 系统变量

  ![image-20220422174734692](https://gitee.com/jackieling/xp01/raw/master/202204221804371.png)

  方便写复杂脚本的时候直接引用这些变量！

+ 自定义变量

  ```shell
  [root@localhost ~]# A=1
  [root@localhost ~]# echo $A
  1
  [root@localhost ~]# 
  ```

  + ​	撤销变量：
  
  ```shell
  [root@localhost ~]# A=1
  [root@localhost ~]# echo $A
  1
  
  ##撤销用unset
  [root@localhost ~]# unset A
  [root@localhost ~]# encho $A
  bash: encho: command not found
  [root@localhost ~]# 
  ```
  
  

+ 声明静态变量以后，变量不能unset：

```shell
##声明静态变量用readonly：
[root@localhost ~]# readonly B=3
[root@localhost ~]# echo $B
3
[root@localhost ~]# unset $B
bash: unset: `3': not a valid identifier
[root@localhost ~]# 


[root@localhost ~]# d="i love you"
[root@localhost ~]# echo $d
i love you
[root@localhost ~]# 
```

## 3.1特殊变量：

## 用于接收shell脚本文件执行时传入的参数：$n

```shell
[root@localhost sh]# bash parameter.sh
[root@localhost sh]# vi parameter.sh
#!/bin/bash

echo $0
echo "输入的第一个参数：$1"
echo "输入的第二个参数：$2"
echo "输入的第11个参数：${11}"

~                                                            
~                                                            
~                                                            
~                                                            
                                                            
"parameter.sh" 7L, 135C written
[root@localhost sh]# bash parameter.sh

[root@localhost sh]# bash parameter.sh 10 20 30 40 50 60 70 80 90 10 22
parameter.sh
输入的第一个参数：10
输入的第二个参数：20
输入的第11个参数：22
```

## 3.2获取所有参数：$#

常用于循环：

会给出参数的数量

```shell
[root@localhost sh]# vi parameter.sh  
#!/bin/bash


echo "输入的第一个参数：$1"
echo "输入的第二个参数：$2"

echo $#
~                                                            
~                                                            
~                                                            
~                                                            
~                                                            
                                                          
"parameter.sh" 7L, 97C written
[root@localhost sh]# bash parameter.sh
输入的第一个参数：
输入的第二个参数：
0
[root@localhost sh]# bash parameter.sh 10 30
输入的第一个参数：10
输入的第二个参数：30
2
```

## 3.3`$*`、`$@`,含义：都是获取所有传入参数，用于后续输出所有参数。

```shell
[root@localhost sh]# bash demo.sh 10 20
直接输出$*:10 20
直接输出$@:10 20
[root@localhost sh]# 
```

## 3.4 $?含义：上一条命令执行状态

```shell
[root@localhost sh]# echo "hello world"
hello world
[root@localhost sh]# echo $?
0
[root@localhost sh]# 
```

得到0就代表执行成功，非0就是不成功.

# 四、运算符

+ 用 `$((运算符))`
+ 用 `expr 运算符 +-*/`

```shell
[root@localhost sh]# vi yunsuan.sh  
#!/bin/bash

echo "第一种运算："
echo $((2+3))

echo "第二种运算："
expr 2 + 3
~                                                            
~                                                            
~                                                            
~                                                            
                                                                                                                
"yunsuan.sh" 7L, 91C written
[root@localhost sh]# bash yunsuan.sh
第一种运算：
5
第二种运算：
5
[root@localhost sh]# 
```

# 五、条件判断

## 5.1常用判断条件

### 1、整数之间的比较

```shell
-eq：等于
-ne：不等于
-gt：大于
-lt：小于
-le：小于等于
-ge：大于等于
 = :字符串比较
```

### 2、按照文件权限进行判断

```shell
-r 文件  判断该文件是否存在，并且是否该文件拥有读权限
-w 文件  判断该文件是否存在，并且是否该文件拥有写权限
-x 文件  判断该文件是否存在，并且是否该文件拥有执行权限
-u 文件  判断该文件是否存在，并且是否该文件拥有SUID权限
-g 文件  判断该文件是否存在，并且是否该文件拥有SGID权限
-k 文件  判断该文件是否存在，并且是否该文件拥有SBit权限
```

### 3、按照文件类型进行判断

![在这里插入图片描述](https://gitee.com/jackieling/xp01/raw/master/202204241837753.png)



### 4、案例实操

<1>、23是否大于等于22

```shell
[root@localhost ~]# [ 23 -ge 22 ]
[root@localhost ~]# echo $?
0
[root@localhost ~]# 
```

<2>、hello.sh是否有写权限

```shell
[root@localhost sh]# [ -w hello.sh ]
[root@localhost sh]# echo $?
0
[root@localhost sh]# 
```

<3>、判断 /home/atguigu/cls.txt 文件是否存在

```shell
[root@localhost ~]# [ -e /home/atguigu/cls.txt ]
[root@localhost ~]# echo $?
0
[root@localhost ~]# 
```

<4>、多条件判断（&& 表示前一条命令执行成功之后才能执行后一条命令）

# 六、流程控制

## 6.1Shell_if判断

```shell
[root@localhost if]# vi if.sh    

#!/bin/bash

if [ $1 -eq 1 ];then
   echo "banzhang zhen"

elif [ $1 -eq 2 ];then
   echo "cls zhenmei"
fi
~                                                            
~                                                            
~                                                            
~                                                            
~                                                            
                                                           
"if.sh" 9L, 108C written
[root@localhost if]# bash if.sh 1
banzhang zhen
[root@localhost if]# bash if.sh 2
cls zhenmei
```



## 6.2Case语句

```shell
[root@localhost sh]# vi case.sh
#!/bin/bash

case $1 in
1)
        echo "banzhang"
;;

2)
        echo "cls"
;;

3)
        echo "renyao"
;;
esac
~                                                            
~                                                            
~                                                            
                                                         
"case.sh" 15L, 93C written
[root@localhost sh]# bash case.sh 2
cls
[root@localhost sh]# 
```



## 6.3For循环

```shell
[root@localhost sh]# vi for.sh  
#!/bin/bash

s=0
for((i=1;i<=100;i++))
do
        s=$[$s+$i]
done
echo $s
~                                                            
~                                                            
~                                                            
~                                                            
~                                                                                                                    
"for.sh" 8L, 67C written
[root@localhost sh]# bash for.sh
5050
[root@localhost sh]# 
```

```shell
[root@localhost sh]# vi for2.sh
#!/bin/bash

for i in $*
do
        echo "banzhangxihuan $i"
done
~                                                            
~                                                            
~                                                            
                                                         
"for2.sh" 6L, 59C written
[root@localhost sh]# ls
batch.sh  demo.sh  for.sh   hello.sh
case.sh   for2.sh  game.sh  yunsuan.sh
[root@localhost sh]# bash for2.sh linghu
banzhangxihuan linghu
[root@localhost sh]# bash for2.sh linghu zhangsan
banzhangxihuan linghu
banzhangxihuan zhangsan
[root@localhost sh]# bash for2.sh linghu zhangsan lisi
banzhangxihuan linghu
banzhangxihuan zhangsan
banzhangxihuan lisi
[root@localhost sh]# 
```

## 6.4While循环

```shell
[root@localhost sh]# vi while.sh
#/bin/bash

s=0
i=1
while [ $i -le 100 ]
do
        s=$[$s + $i]
        i=$[$i + 1]
done
echo $s
~                                                            
~                                                            
                                                                                                                  
"while.sh" 10L, 84C written
[root@localhost sh]# bash while.sh
5050
[root@localhost sh]# 
```

# 七、read读取控制台输入

```shell
[root@localhost sh]# vi read.sh
#/bin/bash

read -t 7 -p "输入你的姓名：" name
echo $name
~                                                            
~                                                            
~                                                            
                                                         
"read.sh" 4L, 65C written
[root@localhost sh]# bash read.sh
输入你的姓名：令狐荣豪
令狐荣豪
[root@localhost sh]# 
```

# 八、函数

## 8.1定义函数与函数调用

```shell
[root@localhost sh]# vi function.sh  
#!/bin/bash

demoFun(){
        echo "这是我的第一个shell函数"

}

echo "=====函数开始执行========"
demoFun
echo "=====函数执行完毕========"
~                                                            
~                                                            
~                                                            
                                                          
"function.sh" 10L, 155C written
[root@localhost sh]# bash function.sh
=====函数开始执行========
这是我的第一个shell函数
=====函数执行完毕========
[root@localhost sh]# 
```

```shell
[root@localhost sh]# vi function.sh  
        echo "输入第一个数字：
echo "输入的两个数字之和为：
#!/bin/bash

demoFun(){
        
        echo "输入第一个数字："
        read aNum
        echo "输入第二个数字："
        read bNum
        return $(($aNum+$bNum))

}

echo "=====函数开始执行========"
demoFun
echo "输入的两个数字之和为：$?"
echo "=====函数执行完毕========"
~                                                            
~                                                            
~                                                            
~                                                            
~                                                            
                                                           
"function.sh" 16L, 272C written
[root@localhost sh]# bash function.sh
=====函数开始执行========
输入第一个数字：
1
输入第二个数字：
2
输入的两个数字之和为：3
=====函数执行完毕========
```



