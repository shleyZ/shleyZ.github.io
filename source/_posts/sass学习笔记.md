---
title: sass学习笔记
date: 2017-09-11 14:06:10
categories: Sass
tags: sass
---

#### Sass 实时编译


- sass --watch <要编译的Sass文件路径>/style.scss:<要输出CSS文件路径>/style.css  


- Grunt 和 Gulp 自动化编译

---

#### Sass 编译常见的错误


- 在Sass的编译的过程中，是不是支持“GBK”编码的。所以在创建 Sass 文件时，就需要将文件编码设置为“utf-8”。
- 路径中的中文字符，建议在项目中文件命名或者文件目录命名不要使用中文字符

---

#### 不同样式风格的输出方法


- 嵌套输出方式 nested
- 展开输出方式 expanded
- 紧凑输出方式 compact 
- 压缩输出方式 compressed

---

#### 默认变量

sass 的默认变量仅需要在值后面加上 !default 即可。

$baseLineHeight:1.5 !default;
body{
line-height: $baseLineHeight;
}

sass 的默认变量一般是用来设置默认值，然后根据需求来覆盖的，覆盖的方式也很简单，只需要在默认变量之前重新声明下变量即可。
默认变量的价值在进行组件化开发的时候会非常有用。

---

#### Sass嵌套


- 选择器嵌套

nav {
a {
color: red;

header & {
color:green;
}
}
}


- 属性嵌套

.box {
border: {
top: 1px solid red;
bottom: 1px solid green;
}
}


- 伪类嵌套

.clearfix{
&:before,
&:after {
content:"";
display: table;
}
&:after {
clear:both;
overflow: hidden;
}
}

编译出来的 CSS：

clearfix:before, .clearfix:after {
content: "";
display: table;
}
.clearfix:after {
clear: both;
overflow: hidden;
}

---

#### 混合宏

- 声明混合宏

@mixin border-radius{
-webkit-border-radius: 5px;
border-radius: 5px;
}


使用“@mixin”来声明一个混合宏。


- 调用混合宏

button {
@include border-radius;
}


关键词“@include”来调用声明好的混合宏

- 混合宏的参数--传多个参数

@mixin center($width,$height){
width: $width;
height: $height;
position: absolute;
top: 50%;
left: 50%;
margin-top: -($height) / 2;
margin-left: -($width) / 2;
}

.box-center {
@include center(500px,300px);
}

有一个特别的参数“…”。当混合宏传的参数过多之时，可以使用参数来替代

@mixin box-shadow($shadows...){
@if length($shadows) >= 1 {
-webkit-box-shadow: $shadows;
box-shadow: $shadows;
} @else {
$shadows: 0 0 2px rgba(#000,.25);
-webkit-box-shadow: $shadow;
box-shadow: $shadow;
}
}


.box {
@include box-shadow(0 0 1px rgba(#000,.5),0 0 2px rgba(#000,.2));
}

- 混合宏的不足

混合宏在实际编码中给我们带来很多方便之处，特别是对于复用重复代码块。但其最大的不足之处是会生成冗余的代码块。

@mixin border-radius{
-webkit-border-radius: 3px;
border-radius: 3px;
}

.box {
@include border-radius;
margin-bottom: 5px;
}

.btn {
@include border-radius;
}


编译出来的 CSS：

.box {
-webkit-border-radius: 3px;
border-radius: 3px;
margin-bottom: 5px;
}

.btn {
-webkit-border-radius: 3px;
border-radius: 3px;
}

不能智能的将相同的样式代码块合并在一起

---

#### Sass扩展/继承

通过关键词 “@extend”来继承已存在的类样式块，从而实现代码的继承。

//SCSS
.btn {
border: 1px solid #ccc;
padding: 6px 10px;
font-size: 14px;
}

.btn-primary {
background-color: #f36;
color: #fff;
@extend .btn;
}

.btn-second {
background-color: orange;
color: #fff;
@extend .btn;
}

编译出来之后：

//CSS
.btn, .btn-primary, .btn-second {
border: 1px solid #ccc;
padding: 6px 10px;
font-size: 14px;
}

.btn-primary {
background-color: #f36;
color: #fff;
}

.btn-second {
background-clor: orange;
color: #fff;
}

#### Sass占位符%

%placeholder 声明的代码，如果不被 @extend 调用的话，不会产生任何代码。

//SCSS
%mt5 {
margin-top: 5px;
}
%pt5{
padding-top: 5px;
}

.btn {
@extend %mt5;
@extend %pt5;
}

.block {
@extend %mt5;

span {
@extend %pt5;
}
}

编译出来的CSS

//CSS
.btn, .block {
margin-top: 5px;
}

.btn, .block span {
padding-top: 5px;
}

#### Sass插值#{}

$properties: (margin, padding);
@mixin set-value($side, $value) {
@each $prop in $properties {
#{$prop}-#{$side}: $value;
}
}
.login-box {
@include set-value(top, 14px);
}


上面的代码编译成 CSS：

.login-box {
margin-top: 14px;
padding-top: 14px;
}

#{}语法并不是随处可用，不能在 mixin 中调用
可以使用 @extend 中使用插值

%updated-status {
margin-top: 20px;
background: #F00;
}
.selected-status {
font-weight: bold;
}
$flag: "status";
.navigation {
@extend %updated-#{$flag};
@extend .selected-#{$flag};
}

上面的 Sass 代码是可以运行的，代码编译出来的 CSS:

.navigation {
margin-top: 20px;
background: #F00;
}
.selected-status, .navigation {
font-weight: bold;
}


#### Sass数据类型

- 数字
- 字符串
- 颜色
- 布尔值
- 空值
- 值列表


#### Sass指令

- @if

//SCSS
@mixin blockOrHidden($boolean:true) {
@if $boolean {
@debug "$boolean is #{$boolean}";
display: block;
}
@else {
@debug "$boolean is #{$boolean}";
display: none;
}
}

.block {
@include blockOrHidden;
}

.hidden{
@include blockOrHidden(false);
}

编译出来的CSS:

.block {
display: block;
}

.hidden {
display: none;
}

- @for循环

@for $i from <start> through <end>
@for $i from <start> to <end>

$i 表示变量，start 表示起始值，end 表示结束值。
关键字 through 表示包括 end 这个数，而 to 则不包括 end 这个数。

through:
@for $i from 1 through 3{
.item-#{$i}{
width:2em * $i;
}
}
编译出来的 CSS:

.item-1 {
width: 2em;
}

.item-2 {
width: 4em;
}

.item-3 {
width: 6em;
}

to:
@for $i from 1 to 3{
.item-#{$i}{
width:2em * $i;
}
}

编译出来的 CSS:

.item-1 {
width: 2em;
}

.item-2 {
width: 4em;
}


- @while

只要 @while 后面的条件为 true 就会执行

- @each

遍历一个列表，然后从列表中取出对应的值

@each $var in <list>



#### Sass函数

- unquote()函数

删除字符串中的引号；

- quote()函数

给字符串添加引号。如果字符串包含引号，不论单引号还是双引号，会统一替换为双引号。
字符串中间有单引号或者空格时，需要用单引号或双引号括起，否则编译的时候将会报错。  
quote() 碰到特殊符号，比如： !、?、> 等，除中折号-和下划线_都需要使用双引号括起，否则编译器在进行编译的时候同样会报错。

.test2 {
content: quote("today is sunday!")
}

- To-upper-case()

函数将字符串小写字母转换成大写字母。

- To-lower-case()

将字符串转换成小写字母


//SCSS
.test {
text: to-upper-case(aaaaa);
text: to-lower-case(aA-aAAA-aaa);
}

- percentage() 数字函数

将一个不带单位的数字转换成百分比形式

.footer{
width : percentage(.2)
}
如果转换的值是一个带有单位的值，那么在编译的时候会报错

- round()函数

将一个数四舍五入为一个最接近的整数

.footer1 {
width:round(15.8px)
}
//CSS
.footer1 {
width: 16px; 
}

- ceil()函数
向上取整

.footer1 {

width:ceil(18.1px);
}


//CSS

.footer1 {

width:19px;
}         

- floor()函数
向下取整

- abs()函数
取绝对值

.footer {

width:abs(-8.9px);
}

//CSS
.footer {

width:8.9px;
}

- min()函数、max()函数

函数中同时出现两种不同类型的单位，将会报错误   

- random()函数

获取一个0-1随机数  

- length()函数 列表函数
- nth()函数 列表函数
>> nth(10px 20px 30px,1)
10px 
//从1开始    

- join()函数 列表函数   

>> join(10px 20px, 30px 40px)
(10px 20px 30px 40px)   
只能将两个列表连接成一个列表，如果直接连接两个以上的列表将会报错

- append()函数 列表函数

将某个值插入到列表中，并且处于最末位

>> append((blue green),red,comma)
(#0000ff, #008000, #ff0000)
>> append((blue green),red,space)
(#0000ff #008000 #ff0000)
>> append((blue, green),red,comma)
(#0000ff, #008000, #ff0000)
>> append((blue, green),red,space)
(#0000ff #008000 #ff0000)
>> append(blue,red,comma)
(#0000ff, #ff0000)
>> append(blue,red,space)
(#0000ff #ff0000)

- zip()函数 列表函数   
将多个列表值转成一个多维的列表

使用zip()函数时，每个单一的列表个数值必须是相同的

>> zip(1px 2px 3px,solid dashed dotted,green blue red)
((1px "solid" #008000), (2px "dashed" #0000ff), (3px "dotted" #ff0000))

- index()函数 列表函数

>> index(1px solid red, 1px)
1
>> index(1px solid red, solid)
2
>> index(1px solid red, red)
3

如果指定的值不在列表中（没有找到相应的值），那么返回的值将是 false，相反就会返回对应的值在列表中所处的位置。

- type-of()  Introspection 函数

判断一个值是属于什么类型：

返回值：

number 为数值型。
string 为字符串型。
bool 为布尔型。
color 为颜色型。        

- unit()函数 Introspection 函数

获取一个值所使用的单位

>> unit(100)
""
>> unit(100px)
"px"
>> unit(20%)
"%"
>> unit(1em)
"em"
>> unit(10px * 3em)
"em*px"
>> unit(10px / 3em)
"px/em"
>> unit(10px * 2em / 3cm / 1rem)
"em/rem"

- unitless()函数 Introspection 函数 

判断一个值是否带有单位，如果不带单位返回的值为 true，带单位返回的值为 false
代码实现：用户在调用混合宏时，如果用户没有给参数值加上单位，程序会自动加入单位。
@mixin adjust-location($x, $y) {
@if unitless($x) {    
$x: 1px * $x;
}
@if unitless($y) {    
$y: 1px * $y;
}
position: relative; 
left: $x; 
top: $y;
}

.botton{
@include adjust-location(20px, 30);
}

- comparable()函数 Introspection 函数

判断两个数是否可以进行“加，减”以及“合并”。如果可以返回的值为 true，如果不可以返回的值是 false
>> comparable(2px,1%)
false
>> comparable(2px,1em)
false
>> comparable(2rem,1em)
false
>> comparable(2px,1cm)
true
>> comparable(2px,1mm)
true


- Miscellaneous函数

if($condition,$if-true,$if-false)
当 $condition 条件成立时，返回的值为 $if-true，否则返回的是 $if-false 值。

>> if(true,1px,2px)
1px
>> if(false,1px,2px)
2px


- map-get($map,$key) Maps的函数     
返回 $key 在 $map 中对应的 value 值。如果 $key 不存在 $map中，将返回 null 值。

$social-colors: (
dribble: #ea4c89,
facebook: #3b5998,
github: #171515,
google: #db4437,
twitter: #55acee
);
.btn-dribble{
color: map-get($social-colors,facebook);
}
如果 $key 不在 $map 中，不会编译出 CSS		

- map-has-key($map,$key)  Maps的函数
函数将返回一个布尔值。当 $map 中有这个 $key，则函数返回 true，否则返回 false。

@function colors($color){
@if not map-has-key($social-colors,$color){
@warn "No color found for `#{$color}` in $social-colors map. Property omitted.";
}
@return map-get($social-colors,$color);
}

.btn-dribble {
color: colors(dribble);
}
.btn-facebook {
color: colors(facebook);
}
.btn-github {
color: colors(github);
}		

- map-keys($map)  Maps的函数
返回 $map 中的所有 key

map-keys($social-colors);


- map-values($map)  Maps的函数
获取的是 $map 的所有 value 值，可以说是一个列表。

map-values($social-colors)

- map-merge($map1,$map2) Maps的函数

map-merge($map1,$map2)	
将 $map1 和 $map2 合并，然后得到一个新的 $map。
如果 $map1 和 $map2 中有相同的 $key 名，那么将 $map2 中的 $key 会取代 $map1 中的

$color: (
text: #f36,
link: #f63,
border: #ddd,
backround: #fff
);
$typo:(
font-size: 12px,
line-height: 1.6,
border: #ccc,
background: #000
);		

$newmap: map-merge($color,$typo);

$newmap:(
text: #f36,
link: #f63,
font-size: 12px,
line-height: 1.6,
border: #ccc,
background: #000
);	

- map-remove($map,$key)  Maps的函数

map-remove($map,$key)

删除当前 $map 中的某一个 $key，从而得到一个新的 map。  
其返回的值还是一个 map。  
他并不能直接从一个 map 中删除另一个 map，仅能通过删除 map 中的某个 key 得到新 map。

- keywords($args)

- RGBA()函数
将一个颜色根据透明度转换成 rgba 颜色

rgba($red,$green,$blue,$alpha)  //将一个rgba颜色转译出来，和未转译的值一样
rgba($color,$alpha)  //将一个Hex颜色转换成rgba颜色

- @import
引入 SCSS 和 Sass 文件。 所有引入的 SCSS 和 Sass 文件都会被合并并输出一个单一的 CSS 文件。 被导入的文件中所定义的变量或 mixins 都可以在主文件中使用。

- @media
在样式中使用 @media 指令，它将冒泡到外面

.sidebar {
width: 300px;
@media screen and (orientation: landscape) {
width: 500px;
}
}
编译出来：

.sidebar {
width: 300px; }
@media screen and (orientation: landscape) {
.sidebar {
width: 500px; } }

- @extend

Sass 中的 @extend 是用来扩展选择器或占位符。比如：

.error {
border: 1px #f00;
background-color: #fdd;
}
.error.intrusion {
background-image: url("/image/hacked.png");
}
.seriousError {
@extend .error;
border-width: 3px;
}
被编译为：

.error, .seriousError {
border: 1px #f00;
background-color: #fdd; }

.error.intrusion, .seriousError.intrusion {
background-image: url("/image/hacked.png"); }

.seriousError {
border-width: 3px; }


- @at-root
跳出根元素

.a {
color: red;

.b {
color: orange;

.c {
color: yellow;

@at-root .d {
color: green;
}
}
}  
}
编译出来的CSS


.a {
color: red;
}

.a .b {
color: orange;
}

.a .b .c {
color: yellow;
}

.d {
color: green;
}

- @debug

@debug 在 Sass 中是用来调试的，当你的在 Sass 的源码中使用了 @debug 指令之后，Sass 代码在编译出错时，在命令终端会输出你设置的提示 Bug:

@debug 10em + 12em;
会输出：

Line 1 DEBUG: 22em

- @warn

@warn 和 @debug 功能类似，用来帮助我们更好的调试 Sass。如：

@mixin adjust-location($x, $y) {
@if unitless($x) {
@warn "Assuming #{$x} to be in pixels";
$x: 1px * $x;
}
@if unitless($y) {
@warn "Assuming #{$y} to be in pixels";
$y: 1px * $y;
}
position: relative; left: $x; top: $y;
}

- @error
@mixin error($x){
@if $x < 10 {
width: $x * 10px;
} @else if $x == 10 {
width: $x;
} @else {
@error "你需要将#{$x}值设置在10以内的数";
}

}

.test {
@include error(15);
}
