---
title: git相关
date: 2018-03-17 14:42:05
categories: git
tags: git
---

## git仓库

1. 创建版本库：
``` js	
	$ mkdir learngit
	$ cd learngit
	$ git init
```
当前目录下多了一个.git的隐藏目录，这个目录是Git来跟踪管理版本库的，千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

2. 把文件添加到版本库

新建一个文件readme.txt

用命令git add告诉Git，把文件添加到仓库（把要提交的所有修改放到暂存区）：

``` js	
$ git add readme.txt
```
用命令git commit告诉Git，把文件提交到仓库（把暂存区的所有修改提交到分支）：

``` js	
$ git commit -m "本次提交的说明"

$ git push origin brachName 把分支上的所有本地提交推送到远程库
```

``` js
$ git status    可以让我们时刻掌握仓库当前的状态

$ git log    显示从最近到最远的提交日志
```

``` js		
commit 8125d1babdf58e7f82b2ce78d048f47d867b1b5c (HEAD -> master)
Author: shleyZ <zxl735975459@126.com>
Date:   Sat Mar 17 11:13:22 2018 +0800

		添加一段内容

commit 43c80b42f0d4da4005a4c7cf87d150d59a44e64a
（HEAD表示当前版本）
```

回退到上一个版本
``` js
$ git reset --hard HEAD^	
```

指定回到某个版本

``` js
$ git reset --hard 3628164(版本号)  
```

查看你之前的每一次命令，前面是版本号前几位

``` js
$ git reflog  
```

		8125d1b (HEAD -> master) HEAD@{0}: commit: 添加一段内容
		43c80b4 HEAD@{1}: commit (initial): 本次提交的说明


删除文件

``` js
$ git rm test.txt  
```


## git分支

创建dev分支，然后切换到dev分支

``` js
$ git checkout -b dev
//Switched to a new branch 'dev'
```

用git branch命令查看当前分支（列出所有分支，当前分支前面会标一个*号）

``` js
$ git branch
* dev
	master
```
合并某分支到当前分支：

``` js		
git merge <name>
```

删除分支：

``` js		
git branch -d <name>

git branch -D <name> （强行删除分支）	
```


## 冲突 

需要根据提示手动解决，最好不要两人在同时对一个文件修改。


## 分支

1. master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

2. 在dev分支干活，也就是说，dev分支是不稳定的

3. 合并分支最好用下面这种方法：

``` js
git merge --no-ff -m "merge with no-ff" dev
```
这样Git就会在merge时生成一个新的commit，能看出来曾经做过合并。

4. bug分支：

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。

5. Feature分支（开发新功能）


## 多人协作：

	git remote -v 查看远程库的信息

	多人协作的工作模式通常是这样：

	首先，可以试图用git push origin branch-name推送自己的修改；

	如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

	如果合并有冲突，则解决冲突，并在本地提交；

	没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

	如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令
	
``` js	
git branch --set-upstream branch-name origin/branch-name。
```
	这就是多人协作的工作模式，一旦熟悉了，就非常简单。


