---
title: html5特性
date: 2017-07-12 15:53:19
categories: html
---

1.新的文档类型

xhtml1.0:
``` html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```
在html5中上面的方法失效：

	<!DOCTYPE html>

2.脚本和链接无需type

在HTML4或XHTML中：
``` html
<link rel="stylesheet" href="path/to/stylesheet.css" type="text/css" />	
<script type="text/javascript" src="path/to/script.js"></script>	
```
在html5中：
``` html
<link rel="stylesheet" href="path/to/stylesheet.css" />	
<script src="path/to/script.js"></script>
```
3.新增语义化标签Header,Footer	,nav,aside,article,section。
``` html
	[<article></article>](http://www.w3school.com.cn/tags/tag_article.asp)   
	##article标签规定独立的自包含内容

	[<aside></aside>](http://www.w3school.com.cn/tags/tag_aside.asp)       
	##aside 的内容应该与附近的内容相关,可用作文章的侧栏。

	[<audio src="">提示</audio>](http://www.w3school.com.cn/tags/tag_audio.asp)   
	##定义声音，比如音乐或其他音频流。

	[<bdi dir=""></bdi>](http://www.w3school.com.cn/tags/tag_bdi.asp)    
	##定义文本的文本方向，使其脱离其周围文本的方向设置。

	[<canvas></canvas>](http://www.w3school.com.cn/tags/tag_canvas.asp)	  
	##定义图形

	[<command></command>](http://www.w3school.com.cn/tags/tag_command.asp)   
	##定义命令按钮，比如单选按钮、复选框或按钮。

	[<datalist></datalist>](http://www.w3school.com.cn/tags/tag_datalist.asp) 
	##datalist 及其选项不会被显示出来，它仅仅是合法的输入值列表。

	[<details></details>](http://www.w3school.com.cn/tags/tag_details.asp)   
	##用于描述文档或文档某个部分的细节。其内部搭配使用summary标签和p标签。

	[<summary></summary>](http://www.w3school.com.cn/tags/tag_summary.asp)   
	##包含 details 元素的标题

	[<dialog></dialog> ](http://www.w3school.com.cn/tags/tag_dialog.asp)    
	##定义对话框或窗口。

	[<embed src="" type="" />](http://www.w3school.com.cn/tags/tag_embed.asp)    
	##定义嵌入的内容，比如插件。

	[<figcaption></figcaption>](http://www.w3school.com.cn/tags/tag_figcaption.asp)  
	 ##定义 figure 元素的标题

	[<figure></figure>](http://www.w3school.com.cn/tags/tag_figure.asp)     
	##规定独立的流内容（图像、图表、照片、代码等等）

	[<footer></footer>](http://www.w3school.com.cn/tags/tag_footer.asp)     
	##页脚通常包含文档的作者、版权信息、使用条款链接、联系信息等等

	[<header></header>](http://www.w3school.com.cn/tags/tag_header.asp)     
	##定义文档的页眉（介绍信息）

	[<keygen></keygen>](http://www.w3school.com.cn/tags/tag_keygen.asp)     
	##规定用于表单的密钥对生成器字段

	[<mark></mark>](http://www.w3school.com.cn/tags/tag_mark.asp)     
	##定义带有记号的文本。

	[<meter></meter>](http://www.w3school.com.cn/tags/tag_meter.asp)     
	##使用 meter 元素来度量给定范围（gauge）内的数据

	[<nav></nav>](http://www.w3school.com.cn/tags/tag_nav.asp)     
	##定义导航链接的部分。

	[<output></output>](http://www.w3school.com.cn/tags/tag_output.asp)     
	##定义不同类型的输出，比如脚本的输出。

	[<progress></progress>](http://www.w3school.com.cn/tags/tag_progress.asp) 
	##标示任务的进度（进程）

	[<rp></rp>](http://www.w3school.com.cn/tags/tag_rp.asp)     
	##支持 "ruby" 元素的浏览器不会显示 "rp" 元素的内容。

	[<rt></rt>](http://www.w3school.com.cn/tags/tag_rt.asp)     
	##定义字符（中文注音或字符）的解释或发音

	[<ruby></ruby>](http://www.w3school.com.cn/tags/tag_ruby.asp)     
	##定义 ruby 注释（中文注音或字符）

	[<section></section>](http://www.w3school.com.cn/tags/tag_section.asp)     
	##文档中的区段

	[<source />](http://www.w3school.com.cn/tags/tag_source.asp)     
	##为媒介元素（比如video和audio）定义媒介资源

	[<time></time>](http://www.w3school.com.cn/tags/tag_time.asp)     
	##定义公历的时间（24 小时制）或日期，时间和时区偏移是可选的。在任何浏览器中不会呈现任何特殊效果

	[<track></track>](http://www.w3school.com.cn/tags/tag_track.asp)     
	##为诸如 video 元素之类的媒介规定外部文本轨道

	[<video src=""></video>](http://www.w3school.com.cn/tags/tag_video.asp)     
	##定义视频，比如电影片段或其他视频流

	[<wbr></wbr>](http://www.w3school.com.cn/tags/tag_wbr.asp)     
	##规定在文本中的何处适合添加换行符
```
4.window.navigator.geolocation








 



