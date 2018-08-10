---
title: shell进行vim操作Found a swp file by the name
date: 2017-12-10 20:15:52
categories: Linux
---
用vim打开.bash_profile时出现：

	ATTENTION  
	Found a swap file by the name ".bash_profile.swp"  
	  
	         owned by: ******   dated: ******  
	         file name: *******  
	         modified: YES  
	         user name: ******   host name: *******  
	         process ID: ****** 
			。。。。。。。

原因是之前vi 操作.bash_profile文件时出现了异常中断，所以在当前目录下产生了一个.bash_profile.swp文件

使用ls -a命令查看可以发现这个隐藏文件。


	rm .bash_profile.swp

这样就不会有	ATTENTION 了