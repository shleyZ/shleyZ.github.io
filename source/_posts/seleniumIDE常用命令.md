---
title: seleniumIDE常用命令
date: 2017-07-13 16:15:53
categories: automated testing
---

1、open（URL）命令
     1、作用：打开指定的URL，URL或者为相对或是绝对的URL
     2、target：要打开的URL，value值为空
     当target为空的时候，将要打开baseURL中填写的页面
     当target不为空切值为相对路径，将打开baseURL+target页面，如，假设baseURL为：https://home.cnblogs.com
     而target为/blog/all/，则执行open命令的时候，将打开：https://home.cnblogs.com/blog/all/
     3、当target以http://开头的时候，将忽略baseURL，直接打开target的网址


2、pause（暂停）
    1、作用：暂停脚本运行
    2、waittime：等待时间，单位ms


3、goBack()
   1,作用：模拟单机浏览器的后退按钮，（2）由于没有参数，所以target和value可不填


4、refresh()
    1、作用：刷新当前页面
    2、由于没有参数，所以target和value可不填


5、click()
    1、作用：单击一个按钮，链接，复选框或者单选按钮
    2、如果该单击事件导致新的页面加载，命令将会加上后缀AndWait,即clickAndWait,或者waitForToLoad命令


6、type
    1、作用：向指定输入域中输入指定的值，也可以为下拉框，复选框，和单选按钮赋值
    2、target：元素的定位表达式
    3、value：要输入的值


7、selec(selectLocator,optionLocator)
    1、作用：模拟人工单击下拉列表框，selectLocator：指向指定选择元素的定位器optionLocator：选项的选择器，（默认为标签）
    2、注意下：选项的定位方式和下拉框的定位方式有所不同，
        选项的定位方式：label和Value
        label=文本值，基于选项的文本进行匹配（默认方式），如label=three
        Value=实际值，基于选项的真实进行匹配的，如：value=4


8、close()
    1、用于模拟用户单击窗口上的关闭按钮
    2、没有参数，target和value可以不填写

