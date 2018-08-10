---
title: python操作文件和目录
date: 2017-12-02 15:28:27
categories: Python
---


如何在python中进行文件和目录操作呢，我们可以在命令行输入操作系统提供的各种命令。

python内置的os模块可以直接调用操作系统提供的接口函数

#### 1.引用os模块

在python交互命令行中

	>>> import os 
	>>> os.name
	'posix'

如果系统是Linux，Unix或Mac OS X，那么os.name是'posix'
如果系统是Windows，那么os.name是'nt'

#### 2.操作系统中定义的环境变量都在os.environ中

	>>> os.environ
	environ({'TERM_PROGRAM': 'Apple_Terminal', 'SHELL': '/bin/bash', 'TERM': 'xterm-256color', 'TMPDIR': '/var/folders/jg/dr9gc8rd7y7g0pzyv3xzzp500000gn/T/', 'Apple_PubSub_Socket_Render': '/private/tmp/com.apple.launchd.LI1PfpIvgk/Render', 'TERM_PROGRAM_VERSION': '38.8.1.1', 'OLDPWD': '/Users/*****/workspace', 'TERM_SESSION_ID': '475******6', 'USER': '*****', 'SSH_AUTH_SOCK': '/private/tmp/com.apple.launchd.h8tpeio0hp/Listeners', '__CF_USER_TEXT_ENCODING': '0x1F5:0x19:0x34', 'PATH': '/anaconda3/bin:/Library/Frameworks/Python.framework/Versions/3.6/bin:/anaconda3/bin:/Library/Frameworks/Python.framework/Versions/3.6/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home/bin:/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home/jre/bin:/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home/bin:/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home/jre/bin', 'PWD': '/Users/*****/workspace/cpython', 'JAVA_HOME': '/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home', 'LANG': 'zh_CN.UTF-8', 'XPC_FLAGS': '0x0', 'XPC_SERVICE_NAME': '0', 'SHLVL': '1', 'HOME': '/Users/*****', 'LOGNAME': '*****', 'CLASSPATH': '.:/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home/lib:.:/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home', '_': '/anaconda3/bin/python3'})

要获取某个环境变量的值，可以用os.environ.get('key'):

	>>> os.environ.get('PATH')
	'/anaconda3/bin:/Library/Frameworks/Python.framework/Versions/3.6/bin:/anaconda3/bin:/Library/Frameworks/Python.framework/Versions/3.6/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home/bin:/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home/jre/bin:/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home/bin:/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home/jre/bin'

#### 3.操作文件和目录

	#查看当前绝对路径
	>>> os.path.abspath('.')
	'/Users/******/Workspace/Cpython'
	
	# 在目录下创建一个新目录seconddir
	>>> os.path.join('/Users/******/Workspace/Cpython','seconddir')
	'/Users/******/Workspace/Cpython/seconddir'
	>>> os.mkdir('/Users/******/Workspace/Cpython/seconddir')

	# 删除一个目录:
	>>> os.rmdir('/Users/******/Workspace/Cpython/seconddir')

    # 拼接路径：
    >>> os.path.join('/Users/******/Workspace','Cpython')
	'/Users/******/Workspace/Cpython'
    
    # 拆分路径：
    >>> os.path.split('/Users/******/Workspace/Cpython')
	('/Users/******/Workspace', 'Cpython')

	# 对文件夹或者文件重命名：
	>>> os.rename('seconddir','second')

	# 删掉文件（当前目录下的）
	>>> os.remove('first_test.py')

    # 列出当前目录下的所有目录
    >>> [x for x in os.listdir('.') if os.path.isdir(x)]
	['__pycache__', 'second']

	# 列出所有的.py文件：
	>>> [x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1] == '.py']
	['first.py', 'first_test.py', 'hello.py', 'learning.py', 'module.py']






