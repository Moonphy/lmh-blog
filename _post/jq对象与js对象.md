# jq对象与js对象
老司机们对DOM对象跟jq对象肯定很熟悉了, 今天突然想起前两天做的一个小项目, 就是在这两对象中突然就搞混了, 用trigger的时候报not a function, 志文哥哥过来瞄了一眼: 这不是jq对象…
## 什么是jq对象跟js对象
要说什么是jq对象首先要知道什么是jq对吧，
jQuery就是js的一个扩展库，工具库，提供很多方便快捷的方法，所以将JS对象转换为jQuery对象后，能更方便地操作这个对象。
但是jQuery对象也不是万能的，有一些JS对象有的功能，jQuery对象并没有提供，所以需要转换回JS对象，才能进行操作。
另外一种情况可能是，你使用某些第三方库，接口函数只能接受JS对象或者jQuery对象，那么你就需要在这两者之间进行转换。
##  jq对象转成DOM对象
两种转换方式将一个jQuery对象转换成DOM对象：[index]和.get(index)。

如：
``` bash
var $e =$("#id") ; //jQuery对象

var e=$e[0]; //DOM对象
var e=$e.get(0); //DOM对象
```
##  DOM对象转成jq对象
对于已经是一个DOM对象，只需要用$()把DOM对象包装起来，就可以获得一个jQuery对象了。
如：
``` bash
var e=document.getElementById("id"); //DOM对象

var $e=$(e); //jQuery对象
```