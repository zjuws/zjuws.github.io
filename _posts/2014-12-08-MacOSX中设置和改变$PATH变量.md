---
layout: post
title: "MacOSX中设置和改变$PATH变量"
description: "Mac"
category: PATH
tags: [PATH]
---
{% include JB/setup %}

###什么是$PATH

$PATH是Linux,OS X,Unix,Windows上的一个环境变量。$PATH变量通过冒号（:）分隔目录地址。如果要打印当前的设置，打开终端输入：

	echo "$PATH"

或者

	printf "%s\n" $PATH
	
###OSX下改变你的PATH环境变量

你可以使用下面的任一方法

1. `$HOME/.bash_profile`文件使用了export语法。
2. `/etc/paths.d`目录

####方法1：`$HOME/.bash_profile`文件
它的语法如下：

	export PATH=$PATH:/new/dir/location1
	export PATH=$PATH:/new/dir1:/dir2:/dir/path/no3

举个例子，添加/usr/local/sbin/mypath这个目录到$PATH变量。编辑`$HOME/.bash_profile`文件，终端输入

	vi $HOME/.bash_profile
	
或者

	vi ~/.bash_profile
	
添加下面的export命令：

	export PATH=$PATH:/usr/local/sbin/mypath
	
保存关闭文件后，如果要立即实现，则输入：

	source $HOME/.bash_profile
	
或者

	. $HOME/.bash_profile
	
最后，验证一下：

	echo $PATH
	
样例输出：

	/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/sbin/mypath
	
####方法2：`/etc/paths.d`目录

苹果推荐使用path_helper工具生成PATH变量，一下是man的介绍：

>The path_helper utility reads the contents of the files in the directories /etc/paths.d and /etc/manpaths.d and appends their contents to the PATH and MANPATH 
environment variables respectively.

>(The MANPATH environment variable will not be modified unless it is already set in the environment.)

>Files in these directories should contain one path element per line.

>Prior to reading these directories, default PATH and MANPATH values are obtained from the files /etc/paths and /etc/manpaths respectively.

列出已存在的path，输入：

	ls -l /etc/paths.d/

样例输出：

	total 16
	-rw-r--r--  1 root  wheel  13 Sep 28  2012 40-XQuartz
	
你可以使用cat命令看一下40-XQuartz的path设置

	cat /etc/paths.d/40-XQuartz
	
样例输出

	/opt/X11/bin

把/usr/local/bin/mypath设置进$PATH,输入：

	sudo -s 'echo "/usr/local/sbin/mypath" > /etc/paths.d/mypath'
	
或者使用像下面一样使用vi指令创建/etc/paths.d/mypath文件：

	sudo vi /etc/paths.d/mypath
	
加入下面内容：

	/usr/local/sbin/mypath
	
保存关闭文件后，你需要重启系统，或者你可以重启终端看一下改变。

###结论
1. 使用$HOME/.bash_profile仅生效于单个用户。
2. 使用etc/paths.d/使在这个系统上的所有用户生效，但此方法仅在OS X Leopard或者更高的系统上生效。
