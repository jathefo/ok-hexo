---
title: 根据不同manifast.xml同步代码
date: 2018-11-26 15:02:14
tags:
---

借助repo同步代码时，可以使用不同的manifest.xml文件替换以达到同步不同代码的目的，这里记下先。

### 1. 首先新建一个存放源码的目录，并进入：
``` bash
$ mkdir SRC
$ cd SRC
```

### 2. 从服务器上下载repo工具：

``` bash
$ git clone git://192.168.1.1/tools/repo.git
```

此时，目录里就有repo文件夹了，里面有repo可执行文件

### 3. 初始化.repo文件夹

``` bash
$ ./repo/repo init -u git://192.168.1.1/branch/manifest.git
```

这时，目录下就了隐藏的.repo文件夹了，在.repo文件夹中，有如下几个文件及目录：

``` bash
drwxr-xr-x 3 ritter ritter 4096 2012-07-23 15:59 manifests
drwxr-xr-x 8 ritter ritter 4096 2012-07-23 15:34 manifests.git
lrwxrwxrwx 1 ritter ritter   21 2012-07-23 16:01 manifest.xml -> manifests/default.xml
drwxr-xr-x 7 ritter ritter 4096 2012-07-23 15:38 repo
```

其中manifest.xml是一个软链接，指向manifests/default.xml。

### 4. 现在，将有下载源码信息的manifest.xml拷入SRC/.repo/manifests/下，并添加执行权限：

``` bash
$ cp manifest.xml SRC/.repo/manifests/ 
$ chmod +x SRC/.repo/manifests/manifest.xml
```

此处注意，如果xml文件名不是manifest.xml也是可以的。

### 5. 然后回到SRC/目录中，执行：

``` bash
$ ./repo/repo init -m manifest.xml
```

使用repo工具，将默认的xml文件从default.xml改成manifest.xml，

### 6. 此时，再到.repo/下查看manifest.xml软链接的目标，已经换成manifests/manifest.xml了：

``` bash
& ls -l SRC/.repo/
drwxr-xr-x 3 ritter ritter 4096 2012-07-23 15:59 manifests
drwxr-xr-x 8 ritter ritter 4096 2012-07-23 15:34 manifests.git
lrwxrwxrwx 1 ritter ritter   21 2012-07-23 16:01 manifest.xml -> manifests/manifest.xml
drwxr-xr-x 7 ritter ritter 4096 2012-07-23 15:38 repo
```

然后，执行repo sync时，同步的代码就是根据新copy的manifest.xml来进行同步的了。
