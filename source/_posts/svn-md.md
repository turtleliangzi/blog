---
layout: post
title: "Linux服务器下搭建svn服务器"
date: 2016-04-18 21:19
comments: true
tags: 
- linux服务器  
- svn
---
在Linux服务器下面配置SVN服务器首先的安装SVN，一般情况下，大多数服务器都安装了svn服务器，其实安装过程也很简单，接下来看看看看怎么配置svn服务器：
**1、在/var/目录下新建一个目录svn：**
``` bash
mkdir svn 
```
**2、进入到svn目录，新建版本库：**
``` bash
svnadmin create 版本库名 
```
> 此处版本库名用admin，可以自行取名，尽量和要导入的文件夹名相同。

<!-- more -->

**3、在admin目录下，配置用户组及用户、密码**
进入authz文件，配置用户组和用户
``` conf
[groups]
用户组 = 用户1、用户2...
...
[/]
@用户组 = rw 或 r 或 w 或无
```
进入passd文件，配置用户密码
``` conf
用户名 = 密码
...
```
**4、在hooks目录下，配置hook钩子**
请执行以下命令
``` bash
cp post-commit.tmpl post-commit
chmod 755 post-commit
```
进入post-commit文件
``` vim
vim post-commit
```
到文件最后（按G键）,删除最后一行，添加以下代码。
```
export LANG=zh_CN.UTF-8
svn up /var/www/文件夹名
```
**5、往版本库中导入内容**
``` svn
svn import  /var/www/要导入的文件夹名  svn://域名或服务器IP地址/版本库名
```
> 此处要导入的文件夹名用admin,版本库名用admin。根据实际情况而定。

**6、进入/var/www目录，将刚刚建立的版本库check out下来**
```svn
rm -rf /var/www/admin
svn co svn://域名或服务器IP地址/admin
```

**7、然后就可以在本地checkout和commit了**
``` svn
svn co svn://域名或服务器地IP址/版本库名称
```
