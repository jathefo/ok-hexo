---
title: cut
date: 2018-11-29 15:32:14
tags:
---

打印文件行的一部分，并将结果显示到标准输出上。

### 用法 
    cut [选项]... [文件]... 

### 选项
    -b, --bytes=列表              只选中指定的这些字节
    -c, --characters=列表         只选中指定的这些字符
    -d, --delimiter=分界符        使用指定分界符代替制表符作为区域分界
    -f, --fields=列表             只选中指定的这些域；并打印所有不包含分界符的
                                  行，除非-s 选项被指定
    -n                            (忽略)
        --complement              补全选中的字节、字符或域
    -s, --only-delimited          不打印没有包含分界符的行
        --output-delimiter=字符串 使用指定的字符串作为输出分界符，默认采用输入
                                  的分界符
    -z, --zero-terminated    line delimiter is NUL, not newline
        --help            显示此帮助信息并退出
        --version         显示版本信息并退出 

### 实例
``` bash
cat test.txt

No name age mark
01 张三 19 91
02 李四 20 87
03 王五 21 98
``` 

使用 -f 选项提取指定字段：
``` bash
cut -f 1 test.txt
结果：
No
01
02
03
``` 
``` bash
cut -f2,3 test.txt
结果：
name age
张三 19
李四 20
王五 21
``` 
--complement 选项提取指定字段之外的列：
``` bash
cut -f3 --complement test.txt
结果：
No name mark
01 张三 91
02 李四 87
03 王五 98
``` 
使用 -d 选项指定字段分隔符：

``` bash
cat test2.txt

No:name:age:mark
01:张三:19:91
02:李四:20:87
03:王五:21:98
``` 

``` bash
cut -f2 -d":" test2.txt
结果：
name
张三
李四
王五
``` 

### 指定字段的字符或者字节范围
N：第N个字节、字符、字段
N-：从第N个字节、字符、字段到结尾
N-M：从第N个字节、字符、字段到第M个（包括M在内）字节、字符、字段
-M：从第1个字节、字符、字段到第M个（包括M在内）字节、字符、字段

上面的范围选择需要结合下面选项使用
-b 表示字节
-c 表示字符
-f 表示定义字段

``` bash
cat test.txt

123456789
123456789
123456789
123456789
123456789
``` 
打印第3个到第5个字符：
``` bash 
cut -c 3-5 test.txt
结果：
345
345
345
345
345
``` 
打印前4个字符：
``` bash
cut -c -4 test.txt
结果：
1234
1234
1234
1234
1234
``` 
打印从第5个字符开始到结尾：
``` bash
cut -c 5- test.txt
结果：
56789
56789
56789
56789
56789
``` 