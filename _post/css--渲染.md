# 基础总结

## 浏览器
### 浏览器解析
浏览器会解析3个东西：
1. DOM
> html/SVG/XHTML，Webkit有三个C++的类对应这三类文档，解析这三种文件会产生一个DOM Tree。
2. CSS
> CSS，解析CSS会产生CSS规则树。
3. javascript
> 主要是通过DOM API和[CSSOM API](https://www.w3.org/TR/cssom-view-1/)来操作DOM Tree和CSS Rule Tree.

### 浏览器构造
解析完，浏览器引擎会通过DOM Tree 和 CSS Rule Tree 来构造 Rendering Tree。

注意
* Rendering Tree 渲染树并不等同于DOM树，因为一些像Header或display:none的东西就没必要放在渲染树中了。
* CSS 的 Rule Tree主要是为了完成匹配并把CSS Rule附加上Rendering Tree上的每个Element。也就是DOM结点。也就是所谓的Frame。
* 然后，计算每个Frame（也就是每个Element）的位置，这又叫layout和reflow过程。

### 浏览器绘制
最后通过调用操作系统Native GUI的API绘制。

## 渲染
渲染的流程基本上如下
1. 计算CSS样式
2. 构建Render Tree
3. Layout – 定位坐标和大小，是否换行，各种position, overflow, z-index属性 ……
4. 正式paint

想要通过css改善性能，一定要清楚两个概念
1. Repaint
>   是屏幕的一部分要重画，比如某个CSS的背景色变了, 但是元素的几何尺寸没有变。
2. Reflow
>   重新，由头再来一次的画，比如元件的几何尺寸变了，我们需要重新验证并计算Render Tree。因为HTML使用的是流式布局， 如果某元件的几何尺寸发生变化， 这时Render Tree的一部分或全部发生了变化， 需要重新布局， 这就是Reflow，或是Layout。


很明显，Reflow的成本比Repaint的成本高得多的多。DOM Tree里的每个结点都会有reflow方法，一个结点的reflow很有可能导致子结点，甚至父点以及同级结点的reflow。在一些高性能的电脑上也许还没什么，但是如果reflow发生在手机上，那么这个过程是非常痛苦和耗电的。

所以，下面这些动作有很大可能会是成本比较高的：
* 当你增加、删除、修改DOM结点时，会导致Reflow或Repaint
* 当你移动DOM的位置，或是搞个动画的时候。
* 当你修改CSS样式的时候。
* 当你Resize窗口的时候（移动端没有这个问题），或是滚动的时候。
* 当你修改网页的默认字体时。

例如：
> display:none会触发reflow，而visibility:hidden只会触发repaint，因为没有发现位置变化。所以，可以用visibility:hidden的时候就尽量不要用display:none。

> 滚屏: 通常来说，如果在滚屏的时候，我们的页面上的所有的像素都会跟着滚动，那么性能上没什么问题，因为我们的显卡对于这种把全屏像素往上往下移的算法是很快。但是如果你有一个fixed的背景图，或是有些Element不跟着滚动，有些Elment是动画，那么这个滚动的动作对于浏览器来说会是相当相当痛苦的一个过程, 很多这样的网页在滚动的时候性能很差， 因为滚屏也有可能会造成reflow。

> 说回前面说的overflow卡顿，当然，很多老司机会说：用-webkit-overflow-scrolling: touch咯。当然通过这种启用硬件加速特性可以使滑动流畅，但通常开启硬件加速也会相应耗费更多内存。我是觉得可以不用overflow的地方尽量不要用，特别是html，body{}这里，我以前很喜欢在这里用hidden，这样很简单很粗暴对不对？完全不用考虑body里面节点是否超过页面造成错乱。但移动端项目做多之后现在很少这样用了至少body不用，一定要用也是在body下面加多层div去用。
	好了，有没有人会好奇，为什么用了-webkit-overflow-scrolling页面就顺畅了，为什么？我也不知道为什么，怎么办，百度知道啊，我不识路——司机知道啊，德性！


