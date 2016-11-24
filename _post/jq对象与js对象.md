# jq对象与js对象
老司机们对DOM对象跟jq对象肯定很熟悉了, 今天突然想起前两天做的一个小项目, 就是在这两对象中突然就搞混了, 用trigger的时候报not a function, 志文哥哥过来瞄了一眼: 这不是jq对象….
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