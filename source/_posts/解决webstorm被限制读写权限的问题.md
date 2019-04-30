---
title: 解决webstorm被限制读写权限的问题
date: 2018-03-15 16:32:28
categories: javascript
---

在webstorm上新建一个项目，发现项目下的所有文件上都多了一个小锁的图标，对这些文件修改或新建文件的时候总是弹出：‘ cannot modify a ready-only directory’的弹窗，

点击修改读写权限的小锁图标，页是弹出‘failed to change read-only flag’。


解决方法：

运行：
``` js
sudo chown -R myusername my-project-folder

```
