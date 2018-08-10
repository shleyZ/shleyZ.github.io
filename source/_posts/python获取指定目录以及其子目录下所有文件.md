---
title: python获取指定目录以及其子目录下所有文件
date: 2017-12-05 16:24:54
categories: Python
---


	#!/usr/bin/env python3
	#-*- coding: utf-8 -*-

	import os

	def findfile(path):

		# 获取指定path下的所有文件和文件夹
		allfiles = os.listdir(path)
		
		for x in allfiles:
			newPath = os.path.join(path,x)

			if os.path.isdir(newPath):  # 如果是文件夹，就继续往下查找
				findfile(newPath)

			if os.path.isfile(newPath):  # 如果是文件，打印出来	
			    print(newPath)	#绝对路径
			    print(newPath.split(path)[1])  # 相对路径

    


